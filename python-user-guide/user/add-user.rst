Add User
========

Add a new user.

.. code-block:: bash

  agate add-user <CREDENTIALS> [OPTIONS] [EXTRA]

.. include:: ../common-credentials.rst

Options
-------

==================================================== ====================================================
Option                                               Description
==================================================== ====================================================
``--name NAME``                                      The user name, required and unique.
``--email EMAIL``                                    The user email, required and unique.
``--upassword UPASSWORD``                            The user password, required if realm is not specified or if it is Agate's one.
``--realm REALM``                                    The realm in which the user will authenticate, optional (default is Agate's realm).
``--first-name FIRST_NAME``                          The user first name.
``--last-name LAST_NAME``                            The user last name.
``--applications [APPLICATIONS [APPLICATIONS ...]]`` The applications in which the user can sign-in, space separated.
``--groups [GROUPS [GROUPS ...]]``                   The groups to which the user belongs, space separated.
``--role ROLE``                                      The role of the user. Default is "agate-user", which gives only the right to user to access to its own profile. Other possible value is "agate-administrator".
``--status STATUS``                                  Only active users can sign-in. Default value is "ACTIVE". Other possible values are: "PENDING", "APPROVED" or "INACTIVE".
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

Add a new user.

.. code-block:: bash

  agate add-user -ag http://localhost:8081 -u administrator -p password --name user1 --email user1@example.org --upassword CHANGEME --applications mica

Add a new user from a Keycloak's server that is registered as as an OpenID Connect realm.

.. code-block:: bash

  agate add-user -ag http://localhost:8081 -u administrator -p password --name user1 --email user1@example.org --realm keycloak --groups mica-user --groups opal-user
