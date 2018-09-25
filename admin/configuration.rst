Configuration
=============

The file **AGATE_HOME/conf/application.yml** is to be edited to match your server needs. This file is written in YAML format allowing to specify a hierarchy within the configuration keys. The YAML format uses indentations to express the different levels of this hierarchy. The file is already pre-filled with default values (to be modified to match your configuration), just be aware that you should not modify the indentations. In the following documentation, the configuration keys will be presented using the dot-notation (levels are separated by dots) for readability.

HTTP Server Configuration
-------------------------

Agate server is a web application and as such, you need to specify on which ports the web server should listen to incoming requests.

=============== ==================
Property        Description
=============== ==================
``server.port`` HTTP port number. Generally speaking this port should not be exposed to the web. Use the https port instead.
``server.host`` Web server host name.
``https.port``  HTTPS port number.
=============== ==================

MongoDB Server Configuration
----------------------------

Agate server will store its data (system configuration, networks, studies, datasets, etc.) in a MongoDB database. You must specify how to connect to this database.

=========================== ===========================
Property                    Description
=========================== ===========================
``spring.data.mongodb.uri`` MongoDB URI. `Read Standard Connection <https://docs.mongodb.com/manual/reference/connection-string/>`_ String Format to learn more.
=========================== ===========================

By default MongoDB does not require any user name, it is highly recommended to configure the database with a user. This can be done by enabling the Client Access Control procedure.

Follow these steps to enable the Client Access Control on your server:

* create a user with the proper roles on the target databases
* restart the MongoDB service with Client Access Control enabled

.. note::

  Once the MongoDB service runs with Client Access Control enabled, all database connections require authentication.

**MongoDB User Creation Example**

The example below creates the *agateadmin* user for *agate* database:

.. code-block:: javascript

  use admin

  db.createUser( {
    user: "agateadmin", pwd: "agateadmin",
    roles: [
         { "role" : "readWrite", "db" : "agate" },
         { "role" : "dbAdmin", "db" : "agate" },
         { "role" : "readAnyDatabase", "db": "admin" }
    ]
  });

Here is the required configuration snippet in **/etc/agate/application.yml** for the above user:

.. code-block:: yaml

  spring:
    data:
      mongodb:
        uri: mongodb://agateadmin:agateadmin@localhost:27017/agate?authSource=admin

.. note::

  Agate requires either **clusterMonitor** or **readAnyDatabase** role on the *admin* database for validation operations. The first role is useful for a cluster setup and the latter if your MongoDB is on a single server.

User Directories
----------------

The security framework that is used by Agate for authentication, authorization etc. is `Shiro <http://shiro.apache.org/>`_. Configuring Shiro for Agate is done via the file **AGATE_HOME/conf/shiro.ini**. See also `Shiro ini file documentation <http://cwiki.apache.org/confluence/display/SHIRO/Configuration#Configuration-INISections>`_.

.. note::

  Default configuration is a static user 'administrator' with password 'password' (or the one provided while installing Agate Debian/RPM package).

By default Agate server has several built-in user directories (in the world of Shiro, a user directory is called a realm):

* a file-based user directory (**shiro.ini** file),
* the internal user directory persisted in the MongoDB database.

Although it is possible to register some additional user directories, this practice is currently not recommended. It is also not recommended to use this file-based user directory for adding users. It is mainly dedicated to define a default system super-user. For a better security, user passwords are encrypted with a one way hash such as sha256. The example **shiro.ini** file below demonstrates how encryption is configured.

.. code-block:: bash

  # =======================
  # Shiro INI configuration
  # =======================

  [main]
  # Objects and their properties are defined here,
  # Such as the securityManager, Realms and anything else needed to build the SecurityManager


  [users]
  # The 'users' section is for simple deployments
  # when you only need a small number of statically-defined set of User accounts.
  #
  # Password here must be encrypted!
  # Use shiro-hasher tools to encrypt your passwords:
  #   DEBIAN:
  #     cd /usr/share/agate/tools && ./shiro-hasher -p
  #   UNIX:
  #     cd <AGATE_DIST_HOME>/tools && ./shiro-hasher -p
  #   WINDOWS:
  #     cd <AGATE_DIST_HOME>/tools && shiro-hasher.bat -p
  #
  # Format is:
  # username=password[,role]*
  administrator = $shiro1$SHA-256$500000$dxucP0IgyO99rdL0Ltj1Qg==$qssS60kTC7TqE61/JFrX/OEk0jsZbYXjiGhR7/t+XNY=,agate-administrator

  [roles]
  # The 'roles' section is for simple deployments
  # when you only need a small number of statically-defined roles.
  # Format is:
  # role=permission[,permission]*
  agate-administrator = *

Passwords must be encrypted using shiro-hasher tools (included in Agate tools directory):

.. code-block:: bash

  cd /usr/share/agate/tools
  ./shiro-hasher -p

Notification Emails
-------------------

Agate offers a notification emails service to the registered applications. Based on email templates, an application can request Agate to send emails to one or more of its users. These templates are defined in the **AGATE_HOME/conf/templates** directory. Agate is using email templates for sending its notifications (email confirmation, reset password etc.).

The email templates specific to an application are located in the directory **AGATE_HOME/conf/templates/<application name>**.

The template engine used for building the email messages is `thymeleaf <http://www.thymeleaf.org/>`_.

Reverse Proxy Configuration
---------------------------

Agate server can be accessed through a reverse proxy server.

**Apache**

Example of Apache directives that:

* redirects HTTP connection on port 80 to HTTPS connection on port 443,
* specifies acceptable protocols and cipher suites,
* refines organization's specific certificate and private key.

.. code-block:: text

  <VirtualHost *:80>
      ServerName agate.your-organization.org
      ProxyRequests Off
      ProxyPreserveHost On
      <Proxy *>
          Order deny,allow
          Allow from all
      </Proxy>
      RewriteEngine on
      ReWriteCond %{SERVER_PORT} !^443$
      RewriteRule ^/(.*) https://agate.your-organization.org:443/$1 [NC,R,L]
  </VirtualHost>
  <VirtualHost *:443>
      ServerName agate.your-organization.org
      SSLProxyEngine on
      SSLEngine on
      SSLProtocol All -SSLv2 -SSLv3
      SSLHonorCipherOrder on
      # Prefer PFS, allow TLS, avoid SSL, for IE8 on XP still allow 3DES
      SSLCipherSuite "EECDH+ECDSA+AESGCM EECDH+aRSA+AESGCM EECDH+ECDSA+SHA384 EECDH+ECDSA+SHA256 EECDH+aRSA+SHA384 EECDH+aRSA+SHA256 EECDH+AESG CM EECDH EDH+AESGCM EDH+aRSA HIGH !MEDIUM !LOW !aNULL !eNULL !LOW !RC4 !MD5 !EXP !PSK !SRP !DSS"
      # Prevent CRIME/BREACH compression attacks
      SSLCompression Off
      SSLCertificateFile /etc/apache2/ssl/cert/your-organization.org.crt
      SSLCertificateKeyFile /etc/apache2/ssl/private/your-organization.org.key
      ProxyRequests Off
      ProxyPreserveHost On
      ProxyPass / https://localhost:8444/
      ProxyPassReverse / https://localhost:8444/
  </VirtualHost>

For performance, you can also activate Apache's compression module (mod_deflate) with the following settings (note the json content type setting) in file */etc/apache2/mods-available/deflate.conf*:

.. code-block:: text

  <IfModule mod_deflate.c>
    <IfModule mod_filter.c>
        # these are known to be safe with MSIE 6
        AddOutputFilterByType DEFLATE text/html text/plain text/xml
        # everything else may cause problems with MSIE 6
        AddOutputFilterByType DEFLATE text/css
        AddOutputFilterByType DEFLATE application/x-javascript application/javascript application/ecmascript
        AddOutputFilterByType DEFLATE application/rss+xml
        AddOutputFilterByType DEFLATE application/xml
        AddOutputFilterByType DEFLATE application/json
    </IfModule>
  </IfModule>
