Setup and Installation
======================

*************
Prerequesites
*************
The main prerequisite is compatible version of `flutter`_ (3.5.3 or later),
and a suitable platform to run the app. 

While the app was designed to target iPhones, the multi-platform nature of flutter means it can be run on other platforms that support local ``sqflite``.

The project also depends on a number of flutter packages. 
These may change as the app undergoes development.
These dependencies are listed in the ``pubspec.yaml`` file:  

.. code-block:: yaml
    
    version: 1.0.0+1

    environment:
        sdk: ^3.5.3

    dependencies:
        flutter:
            sdk: flutter
        intl: ^0.18.0
        cupertino_icons: ^1.0.8
        collection: ^1.18.0
        sqflite: ^2.4.1
        path: ^1.9.0

    dev_dependencies:
        flutter_test:
            sdk: flutter
        test: ^1.19.0
        flutter_launcher_icons: ^0.13.1
        sqflite_common_ffi: ^2.0.0
        mocktail: ^1.0.0

Alternatively, a complete list of dependencies 
can be automatically generated by executing the command ``flutter pub deps`` 
from a terminal in the project directory. 
These dependencies can be automatically installed using ``flutter pub get``. 

*************************
Installation Instructions
*************************
This app does not have any official releases. To run the app, it will be necessary to install from source.
The source code for the app can be cloned from the `github repository`_.
    
The app can be run with by executing ``flutter run`` from the terminal while in the project directory.
(This requires having Flutter installed, as mentioned above.)
Alternatively, some IDEs have flutter support, and so can run the app from the GUI.

.. _github repository: https://github.com/75-Hard-Student-Edition/75-Student
.. _flutter: https://flutter.dev/ 
