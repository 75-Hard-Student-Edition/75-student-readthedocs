State Management
================

Overview
--------

The state management in this application is primarily manual and service-based, utilizing Dart classes and ``setState()`` within Flutter widgets where needed.
The app avoids using external state management libraries such as Provider, Riverpod, or Bloc in favor of a more lightweight and understandable architecture suited to the app's current complexity.

AccountManager
--------------

The ``AccountManager`` class is responsible for managing the user account state, interacting with the database, and handling user-related information.
It maintains a single piece of mutable state:

- ``UserAccountModel? userAccount`` — Holds the current user's account data or ``null`` if not signed in.

**These methods change the internal state:**

- ``login(...)`` — Fetches user data from the database and sets ``userAccount``.
- ``logout()`` — Resets ``userAccount`` to ``null``.
- ``createAccount(...)`` — Creates a new user in the database and sets ``userAccount`` to it.
- ``deleteAccount(...)`` — Deletes the account from the database and resets ``userAccount`` to ``null``.
- ``updateAccount(...)`` — Replaces ``userAccount`` and persists changes.

PointsManager
-------------

The ``PointsManager`` handles internal state for user progress, including total points, completed points, and pass thresholds based on difficulty.
All state—such as ``maxPoints``, ``currentPoints``, ``pointsToPass`` and ``completedTasks``—is self-contained and updated through methods like:

- ``addTask(...)``
- ``completeTask(...)``
- ``uncompleteTask(...)``
- ``removeTask(...)``
- ``calculatePointsToPass()``

ScheduleManager
---------------

The ``ScheduleManager`` class relies on manual, internal state management via class members and callbacks.
It manages the schedule of tasks, including task creation, deletion, and modification.  
It maintains a ``Schedule`` object, which is a list of ``TaskModel`` objects, and exposes methods to manipulate this schedule.  
It's important to note that internally, whenever a ``TaskModel`` is modified, a new object with the modified values is created and the old one is replaced in the ``Schedule`` to avoid mutation.  
It exposes and mutates state via method calls such as:

- ``addTask(...)``
- ``deleteTask(...)``
- ``editTask(...)``
- ``postPoneTask(...)``
- ``completeTask(...)``
- ``uncompleteTask(...)``
- ``scheduleBackLogSuggestion(...)``

**It uses callback functions for UI interaction such as:**

- ``displayErrorCallback(String)`` — Notifies the UI of errors.
- ``userBinarySelectCallback(TaskModel, TaskModel, String)`` — Requests user input on task selection.

Summary
-------

The app uses manual, class-based internal state management, where each manager class maintains its own internal state and exposes methods to mutate or retrieve that state.
