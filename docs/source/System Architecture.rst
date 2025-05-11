System Architecture
===================

The app has a 3-layer architecture. 

******************
Presentation Layer
******************
The GUI is comprised of a collection of "screens" (occasionally "page" is used synomously). 
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

-  ``LoginScreen`` in ``login.dart``

Some screens also have an additional object to implement a navigation bar (NavBar).


********************
Business Logic Layer
********************

The business logic is handled by several "managers". 
These include an ``AccountManager``, a ``ScheduleManager``, an ``AlarmManager``, a ``NotificationManager`` and a ``PointsManager``. 

These managers expose interfaces to the presentation and database layers to use, allowing them to manipulate the internal state of the app, as well as the persistent data in the database.
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
