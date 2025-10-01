Delete Application
==================

Delete an application.

.. code-block:: bash

  agate delete-application <CREDENTIALS> [OPTIONS] [EXTRA]

.. include:: ../common-credentials.rst

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
