Python Introduction
===================

Agate Python client, a command line scripting tool written in Python, enables automation of tasks in a Agate server.

Requirements
------------

Python 3.7+ must be installed on the system. See more about `Python <https://www.python.org/>`_.

Installation
------------

The Agate Python Client is available on the official `Python Package Index <https://pypi.org/>`_.

.. code-block:: bash

  sudo pip install obiba-agate

.. note::
  Previous versions were available as system packages. Make sure to remove them before installing the package with ``pip``.

  .. code-block:: bash

    # on Debian systems
    sudo apt-get remove agate-python-client

    # on RPM systems
    sudo yum remove agate-python-client

.. note::

  This python package depends on the `pycurl <https://pypi.org/project/pycurl/>`_ package which has some system dependencies. One simple solution
  is to install the ``pycurl`` system package before.

  .. code-block:: bash

    # on Debian systems
    sudo apt-get install python3-pycurl

    # on RPM systems
    sudo yum install python3-pycurl


Usage
-----

To get the options of the command line:

.. code-block:: bash

  agate --help

This command will display which sub-commands are available. Further, given a subcommand obtained from command above, its help message can be displayed via:

.. code-block:: bash

  agate <subcommand> --help

This command will display available subcommands.
