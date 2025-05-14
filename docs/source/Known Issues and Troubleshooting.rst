Known Issues and Troubleshooting
================================

See also :doc:`Future Improvements` for a list of features that have not yet been implemented.

************
Known Issues
************

1. Progress bar display doesn't update reactively when tasks are modified - only rebuilt on page rebuild

2. Lack of cloud storage / accounts are entirely local. This means accounts cannot be accessed on different devices and may be unintentionally deleted if the app's cache is wiped.

3. Inconsistent data management between backend and database around the time to notify a user before their task 

4. No frontend option to make a task immovable

5. No way for user to access tasks from backlog

6. Backlog data are not persistent (i.e. not saved to database)

7. Method ``GenerateNewSchedule`` is never called because there is no mechanism in place to detect a change of day

8. Sensitive data (i.e. passwords) are unencrypted. 

9. ``ScheduleManager`` CRUD methods do not await the ``DatabaseService`` methods that they call - results in broken unit tests

