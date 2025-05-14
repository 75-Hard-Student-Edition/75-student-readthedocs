System Architecture
===================

The app has a 3-layer architecture. (Presentation Layer, Business Logic Layer, Database Layer)

******************
Presentation Layer
******************
This code deals with displaying the GUI and handling user input. 
Where the user actions may require some kind of data processing, 
the code in this layer will call methods implemented by the business logic layer.

The GUI is comprised of a collection of "screens" (occasionally "page" is used synonymously). 
The code for each screen is written in its own file in ``lib/userInterfaces``. 
Each screen is comprised of between 1 and 3 objects, depending on the nature of the screen. 
Screens which do not store state are comprised primarily of 1 object,
which extends the ``StatelessWidget`` class. 
Such screens include:

- ``ProfileScreen`` in ``profile.dart``

- ``SettingsPage`` in ``settings_page.dart``

- ``WelcomeScreen`` in ``start_up.dart``

``TaskDetails`` in ``task_details.dart`` is similar to the other screens mentioned in this category, but it is not a full screen. 
It overlays a portion of the home screen (``ScheduleScreen``). 

Screens which store state are primarily comprised of 2 objects. 
The first extends the ``StatefulWidget`` class, and the second extends the ``State<ScreenName>`` class 
(where ``ScreenName`` is the name of that screen's first object.) 
Such screens include:

- ``AddTaskScreen`` in ``add_task.dart``

- ``CategoryRankingScreen`` in ``category_ranking.dart``

- ``DifficultyPage`` in ``difficulty_page.dart``

- ``ScheduleScreen`` in ``home.dart`` (this is the home screen)

- ``MindfulnessScreen`` in ``mindfulness_page.dart``

- ``NotificationScreen`` in ``notification_page.dart``

- ``SignUpScreen`` in ``sign_up.dart``

- ``LoginScreen`` in ``login.dart``

Some screens also have an additional object to implement a navigation bar (NavBar).

More information about the GUI on the :doc:`App Flow and Features` page.

********************
Business Logic Layer
********************
The business logic is primarily handled by several **managers**.
These include:

- ``AccountManager``

- ``ScheduleManager``

- ``AlarmManager``

- ``NotificationManager``

- ``PointsManager``. 

These managers expose interfaces to the presentation layer to use, 
allowing it to manipulate the internal state of the app, 
as well as the persistent data in the database.
This approach simplifies development and testing as it provides a consistent API to be used by the presentation layer.

A more detailed breakdown on how each of the managers works is provided in the :doc:`State Management` section, 
however here is an example of the ScheduleManager interface:

.. code-block:: dart

    abstract class IScheduleManager {
    // ScheduleManager -> GUI methods
    Schedule get schedule;
    List<TaskModel> getBacklogSuggestions();
    Future<TaskModel?> userBinarySelect(TaskModel task1, TaskModel task2, String message);
    void displayError(String message);

    // GUI -> ScheduleManager methods
    void addTask(TaskModel task);
    void deleteTask(int taskId);
    void editTask(TaskModel task);
    void postPoneTask(int taskId);
    void completeTask(int taskId);
    void uncompleteTask(int taskId);
    void scheduleBacklogSuggestion(int taskId);
    DateTime? findAvailableTimeSlot(TaskModel task);

    // ScheduleManager -> ScheduleGenerator methods
    AccountManager get accManager;
    }

**************
Database Layer
**************

This code creates a database and implements CRUD operations for it,
providing an interface for those CRUD operations such that they can be used by the logic layer.

The database is implemented using `SQLite`_, a pared-down database engine,
and `sqflite`_, the flutter package which creates an interface between flutter and SQLite.
Another package, `sqflite_darwin`_ is needed to ensure compatibility with iOS systems.

The SQLite code for creating the tables (seen below) is contained in ``assets/create_tables.sql``,
and the flutter interface (implemented using sqflite) is found in ``lib/database``. 
SQLite stores a database as a single file (in our case ``75_student_db.db``) locally on the user's machine, 
with the directory being dependent on the platform. 
See `this sqflite documentation page <https://github.com/tekartik/sqflite/blob/master/sqflite/doc/opening_db.md#finding-a-location-path-for-the-database>`_ 
for details.

At this stage in the development, the database is less complex than was initially planned,
because some features have yet to be implemented. 
As such it consists of only two tables:

.. code-block:: SQL

    CREATE TABLE IF NOT EXISTS "user" (
        user_id INTEGER PRIMARY KEY AUTOINCREMENT,
        username TEXT NOT NULL,                 
        email TEXT UNIQUE,            
        phone_number TEXT UNIQUE,
        password TEXT,              
        streak INT DEFAULT 0,
        difficulty INTEGER NOT NULL,
        category_order TEXT NOT NULL,
        sleep_duration_minutes INT NOT NULL,
        bedtime TEXT NOT NULL,
        notify_time_minutes INT NOT NULL,
        mindfulness_minutes INT NOT NULL            
    );

    CREATE TABLE IF NOT EXISTS "task" (
        task_id INTEGER PRIMARY KEY AUTOINCREMENT,
        user_id INTEGER REFERENCES "user"(user_id) ON DELETE CASCADE,
        title TEXT NOT NULL, 
        description TEXT NOT NULL, 
        is_moveable INTEGER DEFAULT 0, -- SQLite doesn't have a boolean type
        is_complete INTEGER DEFAULT 0, 
        category INTEGER NOT NULL,
        priority INTEGER NOT NULL,
        start_time TEXT NOT NULL,
        duration_minutes INT NOT NULL, 
        repeat_period TEXT, 
        links TEXT
    )

One of the difficulties of using SQLite is handling its lack of datatypes. 
This means that when data is inserted/retrieved from the database it needs to be serialised/deserialised
from a dart-datatype to an SQLite-compatible-type (particularly, ``TEXT``).
This functionality is provided by an extension on the ``TaskModel`` and ``UserAccountModel``.
The extension on ``TaskModel`` is included below for reference.

.. code-block:: dart

    extension TaskModelDB on TaskModel {
        Map<String, dynamic> toMap(int userId) {
            return {
            "task_id": id,
            "user_id": userId,
            "title": name,
            "description": description,
            "is_moveable": isMovable ? 1 : 0,
            "is_complete": isComplete ? 1 : 0,
            "category": TaskCategory.values.indexOf(category),
            "priority": TaskPriority.values.indexOf(priority),
            "start_time": startTime.toIso8601String(),
            "duration_minutes": duration.inMinutes,
            "repeat_period": period?.inDays.toString() ?? "",
            "links": links ?? ""
            };
        }

        static TaskModel fromMap(Map<String, dynamic> map) {
            print("TaskModelDB.fromMap: ${map.toString()}");
            final ret = TaskModel(
                id: map["task_id"],
                name: map["title"],
                description: map["description"],
                isMovable: map["is_moveable"] == 1,
                isComplete: map["is_complete"] == 1,
                category: TaskCategory.values[map["category"]],
                priority: TaskPriority.values[map["priority"]],
                startTime: DateTime.parse(map["start_time"]),
                duration: Duration(minutes: map["duration_minutes"]),
                period: (map["repeat_period"] != null && map["repeat_period"].toString().isNotEmpty)
                    ? Duration(days: int.parse(map["repeat_period"].toString()))
                    : null,
                links: map["links"]);
            print("TaskModelDB.fromMap: ${ret.toString()}");
            return ret;
        }
    }

.. _SQLite: https://www.sqlite.org/ 
.. _sqflite: https://pub.dev/packages/sqflite
.. _sqflite_darwin: https://pub.dev/packages/sqflite_darwin