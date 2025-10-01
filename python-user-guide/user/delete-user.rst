Delete User
===========

Delete a user.

.. code-block:: bash

  agate delete-user <CREDENTIALS> [OPTIONS] [EXTRA]

.. include:: ../common-credentials.rst

Options
-------

=================== ===================
Option              Description
=================== ===================
``--name NAME``     The user name, mutually exclusive with email.
``--email EMAIL``   The user email, mutually exclusive with name.
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

Delete a user by its name.

.. code-block:: bash

  agate delete-user -ag http://localhost:8081 -u administrator -p password --name user1
