Error Handling and Debugging
============================

This document outlines the standard practice for error handling and debugging within the application. These practices ensure that errors are managed gracefully, without affecting the user experience.

Error Handling
--------------

Use try-catch blocks to handle exceptions, especially for user input operations such as:

*   Moving a task to overlap on another task
*   Loging into an account that doesn't exist
*   Deleting a task that doesn't exist

Caught exceptions should be handled quietly, without crashing the app.

**Custom exceptions:**

*   Whenever possible, define custom exceptions for specific error scenarios.

.. code-block:: dart

    class AccountNotFoundException implements Exception {
      final String message;
      AccountNotFoundException(this.message);

      @override
      String toString() => 'AccountNotFoundException: $message';
    }

**Example:**

.. code-block:: dart

    onPressed: () async {
      // Log in to the account using the AccountManager
      try {
        await widget.accountManager.login(
          _usernameController.text,
          _passwordController.text,
        );
      } on AccountNotFoundException catch (e) {
        // Handle login error (e.g., show a dialog or snackbar)
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(
            content: Text('Login failed: $e'),
            backgroundColor: Colors.red,
          ),
        );
        return;
      }
    }


