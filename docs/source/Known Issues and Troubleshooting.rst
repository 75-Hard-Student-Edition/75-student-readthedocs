.. _known issues:

Known Issues and Troubleshooting
================================
.. note:: This page is currently under development. Please check back later for updates.

************
Known Issues
************

1. Progress bar doesn't always update reactively (needs elaboration)

2. Lack of cloud storage / accounts are entirely local.
This means accounts cannot be accessed on different devices
and may be unintentionally deleted if the app's cache is wiped.

3. Inconsistent data management around taskNotifyBefore
and bedtimeNotifyBefore between the 3 layers (needs elaboration)

4. In backend / database tasks can be "immovable" (does business logic accomodate this?) but there is no option 
in the front end to configure this

5. No way for user to access tasks from backlog

6. Backlog data is not persistent (i.e. not saved to database)

7. Method ``GenerateNewSchedule`` is never called because
there is no mechanism in place to detect a change of day

8. Sensitive data (i.e. passwords) is unencrypted. 

TODO elaborate on "Troubleshooting" part

