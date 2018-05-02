Add Application
===============

Add a new application.

.. code-block:: bash

  agate add-application <CREDENTIALS> [OPTIONS] [EXTRA]

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

=============================== ====================================================
Option                          Description
=============================== ====================================================
``--name NAME``                 The application name, required and unique.
``--description DESCRIPTION``   The description of the application, optional.
``--key KEY``                   The application key, required.
``--redirect REDIRECT``         Callback URL to the application's server, required in the OAuth context.
=============================== ====================================================

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

Add a new application.

.. code-block:: bash

  agate add-application -ag http://localhost:8081 -u administrator -p password --name someapp --key ABCDEFGH1234
