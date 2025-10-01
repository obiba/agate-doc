Delete Group
============

Delete a group.

.. code-block:: bash

  agate delete-group <CREDENTIALS> [OPTIONS] [EXTRA]

.. include:: ../common-credentials.rst

Options
-------

=================== ===================
Option              Description
=================== ===================
``--name NAME``     The group name, required.
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

Delete a group.

.. code-block:: bash

  agate delete-group -ag http://localhost:8081 -u administrator -p password --name researchers
