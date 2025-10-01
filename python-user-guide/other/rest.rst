Web Services
============

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

.. include:: ../common-credentials.rst

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
