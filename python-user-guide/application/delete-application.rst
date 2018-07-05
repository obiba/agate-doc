Delete Application
==================

Delete an application.

.. code-block:: bash

  agate delete-application <CREDENTIALS> [OPTIONS] [EXTRA]

Credentials
-----------

Authentication is done by username/password credentials.

==================================== ====================================
Option                               Description
==================================== ====================================
``--agate AGATE, -ag AGATE``         Agate server base url.
``--user USER, -u USER``             User name. User with appropriate permissions is expected depending of the REST resource requested.
``--password PASSWORD, -p PASSWORD`` User password.
==================================== ====================================

Options
-------

=================== ===================
Option              Description
=================== ===================
``--name NAME``     The application name, required.
=================== ===================

Extras
------

================= =================
Option            Description
================= =================
``-h, --help``    Show the command help's message
``--verbose, -v`` Verbose output
================= =================

Example
-------

Delete an application.

.. code-block:: bash

  agate delete-application -ag http://localhost:8081 -u administrator -p password --name someapp
