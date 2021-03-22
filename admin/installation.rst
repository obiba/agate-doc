Installation
============

Agate is a stand-alone `Java <https://www.java.com>`_ server application that requires `MongoDB <https://www.mongodb.com/>`_ as database engine.

Requirements
------------

Server Hardware Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

============ ===============
Component    Requirement
============ ===============
CPU	         Recent server-grade or high-end consumer-grade processor
Disk space	 8GB or more.
Memory (RAM) Minimum: 4GB, Recommended: >4GB
============ ===============

Server Software Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

======== ================= ================================================================ ========================
Software Suggested version Download link                                                    Usage
======== ================= ================================================================ ========================
Java     8                 `OpenJDK 8 downloads <http://openjdk.java.net/projects/jdk8/>`_  Java runtime environment
MongoDB  >= 2.4.x          `MongoDB downloads <http://www.mongodb.org/downloads>`_          Database engine
======== ================= ================================================================ ========================

While Java is required by Agate server application, MongoDB can be installed on another server.

Install
-------

Agate is distributed as a Debian/RPM package and as a zip file. The resulting installation has default configuration that makes Agate ready to be used (as soon as a MongoDB server is available). Once installation is done, see :doc:`configuration` instructions.

Debian Package Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Agate is available as a Debian package from OBiBa Debian repository. To proceed installation, do as follows:

* `Install Debian package <http://www.obiba.org/pages/pkg/>`_. Follow the instructions in the repository main page for installing Agate.
* Manage Agate Service: after package installation, Agate server is running: see how to manage the Service.

RPM Package Installation
~~~~~~~~~~~~~~~~~~~~~~~~

Agate is available as a RPM package from OBiBa RPM repository. To proceed installation, do as follows:

* `Install RPM package <http://www.obiba.org/pages/rpm/>`_. Follow the instructions in the RPM repository main page for installing Agate.
* Manage Agate Service: after package installation, Agate is running: see how to manage the Service.

Zip Distribution Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Agate is also available as a Zip file. To install Agate zip distribution, proceed as follows:

* `Download Agate distribution <https://github.com/obiba/agate/releases>`_
* Unzip the Agate distribution. Note that the zip file contains a root directory named **agate-x.y.z-dist** (where x, y and z are the major, minor and micro releases, respectively). You can copy it wherever you want. You can also rename it.
* Create an ``AGATE_HOME`` environment variable
* Separate Agate home from Agate distribution directories (recommended). This will facilitate subsequent upgrades.

Set-up example for Linux:

.. code-block:: bash

  mkdir agate-home
  cp -r agate-x-dist/conf agate-home
  export AGATE_HOME=`pwd`/agate-home
  ./agate-x-dist/bin/agate

Launch Agate. This step will create/update the database schema for Agate and will start Agate: see Regular Command.

For the administrator accounts, the credentials are "administrator" as username and "password" as password. See User Directories Configuration to change it.

Docker Image Installation
~~~~~~~~~~~~~~~~~~~~~~~~~

OBiBa is an early adopter of the `Docker <https://www.docker.com/>`_ technology, providing its own images from the `Docker Hub repository <https://hub.docker.com/orgs/obiba/repositories>`_.

A typical `docker-compose <https://docs.docker.com/compose/>`_ file (including a MongoDB database) would be:

.. code-block:: yaml

  version: '3'
  services:
      agate:
          image: obiba/agate
          ports:
                  - "8844:8444"
                  - "8881:8081"
          links:
                  - mongo
          environment:
                  - AGATE_ADMINISTRATOR_PASSWORD=password
                  - MONGO_HOST=mongo
                  - MONGO_PORT=27017
                  - RECAPTCHA_SITE_KEY=6Lfo7gYTAAAAAOyl8_MHuH-AVBzRDtpIuJrjL3Pb
                  - RECAPTCHA_SECRET_KEY=6Lfo7gYTAAAAADym-vSDvPBeBCXaxIprA0QXLk_b
          volumes:
                  - /tmp/agate:/srv

Then environment variables that are exposed by this image are:

================================= =========================================================================
Environment Variable              Description
================================= =========================================================================
``JAVA_OPTS``
``AGATE_ADMINISTRATOR_PASSWORD``  Agate administrator password, required and set at first start.
``MONGO_HOST``                    MongoDB server host (optional).
``MONGO_PORT``                    MongoDB server port, default is ``27017``.
``RECAPTCHA_SITE_KEY``            `reCAPTCHA v2 <https://developers.google.com/recaptcha>`_ site key
``RECAPTCHA_SECRET_KEY``          `reCAPTCHA v2 <https://developers.google.com/recaptcha>`_ secret key
================================= =========================================================================

Upgrade
-------

The upgrade procedures are handled by the application itself.

Debian Package Upgrade
~~~~~~~~~~~~~~~~~~~~~~

If you installed Agate via the Debian package, you may update it using the command:

.. code-block:: bash

  apt-get install agate

RPM Package Upgrade
~~~~~~~~~~~~~~~~~~~

If you installed Agate via the RPM package, you may update it using the command:

.. code-block:: bash

  yum install agate

Zip Distribution Upgrade
~~~~~~~~~~~~~~~~~~~~~~~~

Follow the Installation of Agate Zip distribution above but make sure you don't overwrite your agate-home directory.

Execution
---------

Server launch
~~~~~~~~~~~~~

**Service**

When Agate is installed through a Debian/RPM package, Agate server can be managed as a service.

Options for the Java Virtual Machine can be modified if Agate service needs more memory. To do this, modify the value of the environment variable ``JAVA_ARGS`` in the file **/etc/default/agate**.

Main actions on Agate service are: ``start``, ``stop``, ``status``, ``restart``. For more information about available actions on Agate service, type:

.. code-block:: bash

  service agate help

The Agate service log files are located in **/var/log/agate** directory.

**Manually**

The Agate server can be launched from the command line. The environment variable ``AGATE_HOME`` needs to be setup before launching Agate manually.

==================== ======== ===========
Environment variable Required Description
==================== ======== ===========
``AGATE_HOME``       yes      Path to the Agate "home" directory.
``JAVA_OPTS``        no       Options for the Java Virtual Machine. For example: `-Xmx4096m -XX:MaxPermSize=256m`
==================== ======== ===========

To change the defaults update:  ``bin/agate`` or ``bin/agate.bat``

Make sure Command Environment is setup and execute the command line (bin directory is in your execution PATH)):

.. code-block:: bash

  agate

Executing this command upgrades the Agate server and then launches it.

The Agate server log files are located in **AGATE_HOME/logs** directory. If the logs directory does not exist, it will be created by Agate.

Usage
~~~~~

To access Agate with a web browser the following urls may be used (port numbers may be different depending on HTTP Server Configuration):

* http://localhost:8081 will provide a connection without encryption,
* https://localhost:8444 will provide a connection secured with ssl.

Troubleshooting
~~~~~~~~~~~~~~~

If you encounter an issue during the installation and you can't resolve it, please report it in our `Agate Issue Tracker <https://github.com/obiba/agate/issues>`_.

Agate logs can be found in **/var/log/agate**. If the installation fails, always refer to this log when reporting an error.
