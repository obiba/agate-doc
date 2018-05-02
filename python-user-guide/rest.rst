REST API
========

This command is for advanced users wanting to directly access to the REST API of Agate server.

.. code-block:: bash

  agate rest ws <CREDENTIALS> [OPTIONS] [EXTRA]

Arguments
---------

======== ===========
Argument Description
======== ===========
``ws``	 Web service path, for instance: /user/xxx
======== ===========

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

================================================= ====================================
Option                                            Description
================================================= ====================================
``--method METHOD, -m METHOD``                    HTTP method: GET (default), POST, PUT, DELETE, OPTIONS.
``--accept ACCEPT, -a ACCEPT``                    Accept header (default is application/json).
``--content-type CONTENT_TYPE, -ct CONTENT_TYPE`` Content-Type header (default is application/json).
``--json, -j``                                    Pretty JSON formatting of the response.
================================================= ====================================

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

Get all users.

.. code-block:: bash

  agate rest -ag http://localhost:8081 -u administrator -p password -j /users
