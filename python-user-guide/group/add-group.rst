Add Group
=========

Add a new group.

.. code-block:: bash

  agate add-group <CREDENTIALS> [OPTIONS] [EXTRA]

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

==================================================== ====================================================
Option                                               Description
==================================================== ====================================================
``--name NAME``                                      The group name, required and unique.
``--description DESCRIPTION``                        The description of the group, optional.
``--applications [APPLICATIONS [APPLICATIONS ...]]`` The applications in which the users members of the group can sign-in, space separated.
==================================================== ====================================================

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

Add a new group.

.. code-block:: bash

  agate add-group -ag http://localhost:8081 -u administrator -p password --name researchers --applications mica drupal
