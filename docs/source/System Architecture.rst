System Architecture
===================
.. note:: This page is currently under development. Please check back later for updates.

The app has a 3-layer architecture. (Presentation Layer, Business Logic Layer, Database Layer)

******************
******************
Presentation Layer
******************
This code deals with displaying the GUI and handling user input. 
Where the user actions may require some kind of data processing, 
the code in this layer will call methods implemented by the business logic layer.

The GUI is comprised of a collection of "screens" (occasionally "page" is used synomously). 
The code for each screen is written in its own file in ``lib/userInterfaces``. 
Each screen is comprised of between 1 and 3 objects, depending on the nature of the screen. 

Screens which do not store state are comprised primarily of 1 object,
which extends the ``StatelessWidget`` class. 
Such screens include:

- ``LogInScreen`` in ``log_in.dart``

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

Some screens also have an additional object to implement a navigation bar (NavBar).

More information about the GUI on the :ref:`App Flow and Features` page.

********************
********************
Business Logic Layer
********************
This code deals with processing data. 
It implements several methods which may be called by the presentation layer.
When data needs to be permanently stored or retrieved, 
the code in this layer will call methods implemented by the database layer.

The business logic is primarily handled by several **managers**.
These include:

- ``AccountManager``

- ``ScheduleManager``

- ``AlarmManager``

- ``NotificationManager``

- ``PointsManager``. 

The code for these can be found in ``lib/Components``. 

The ``AccountManager`` and ``ScheduleManager``, being particularly complex, are divided across several files. 
These managers are defined by an interface, which specifies the methods they provide. 
This simplifies development as it provides a consistent API to be used by the presentation layer.

TODO: Breakdown of functionality / interaction of these different managers?

**************
Database Layer
**************
This code (found in ``lib/database``) deals with the storage, retrieval and updating of data in the database. 
It implements several methods which may be called by the business logic layer. 
TODO expand what methods exactly. Also for other layers expand what methods exactly.

The database is implemented using SQLite and the flutter package for handling SQLite, ``sqflite``. 
Another package, ``sqflite_darwin`` is needed to ensure compatibility with iOS systems.

