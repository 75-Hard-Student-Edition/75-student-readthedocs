Testing
=======

*****************
Automated Testing
*****************

This project has a suite of automated unit tests, written in the ``test`` directory. 
These automated tests are written using the `flutter_test`_ or `test`_ packages. 
The `mocktail`_ package is also used to create mocks. 

There is unfortunately no continuous integration, however the test files can be run 
by executing ``flutter test <test-file-path>`` in the terminal. 
Running the whole test suite will require running the above command for the each
test file. 

Alternatively, depending on your IDE, their may be a graphical way to run the tests. 
In VSCode, the development environment of choice for our team, this appears as a 
green "play" button next to each test (to run that test)
or a similar icon next to the test group (to run the entire group).

**************
System Testing
**************

During development thus far, manual system testing has also been used to test
compliance with the system requirements (including identifying those areas in which the system fails). 
This testing has identified a number of issues with the system, which can be found on 
the :doc:`Known Issues and Troubleshooting` page.

There are no manual test cases.

.. _mocktail: https://pub.dev/packages/mocktail
.. _flutter_test: https://api.flutter.dev/flutter/flutter_test/ 
.. _test: https://pub.dev/packages/test 