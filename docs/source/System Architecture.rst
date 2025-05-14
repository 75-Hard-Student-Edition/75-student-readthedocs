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

More information about the GUI on the :ref:`App Flow and Features` page.

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

These managers expose interfaces to the presentation and database layers to use, 
allowing them to manipulate the internal state of the app, 
as well as the persistent data in the database.
This approach simplifies development and testing as it provides a consistent API to be used by the presentation layer.

A more detailed breakdown on how each of the managers works is provided in the :doc:`State Management` section, however here is an example of the ScheduleManager interface:

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
This code (found in ``lib/database``) deals with the storage, retrieval and updating of data in the database. 
It implements several methods which may be called by the business logic layer. 
TODO expand what methods exactly. Also for other layers expand what methods exactly.

The database is implemented using `SQLite`_ and the flutter package for handling SQLite, ``sqflite``. 
Another package, ``sqflite_darwin`` is needed to ensure compatibility with iOS systems.

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
    );

.. _SQLite: https://www.sqlite.org/ 