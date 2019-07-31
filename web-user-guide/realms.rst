Realms Management
=================

A Realm is a data access object that provides authentication capabilities for Agate users using the underlying datasource's specific API to discover authorization data.

Realm Types
-----------

LDAP Realm
~~~~~~~~~~

A realm that authenticates users by using Lightweight Directory Access Protocol to query a Directory Access Agent.
This realm uses a user's Distinguished Name (DN) template to build queries.

Active Directory Realm
~~~~~~~~~~~~~~~~~~~~~~

A realm tailored to a Microsoft Active Directory environment.
This realm queries by using a combination of a search filter and search base.

SQL Database Realm
~~~~~~~~~~~~~~~~~~

``mysql``, ``mariadb`` and ``postgresql`` are supported.
This realm queries the user's password with the salt style used by the database.

Salt styles include:

- ``NO_SALT``: used when the password is in plain text.
- ``CRYPT``: uses the database's underlying cryptographic method to decrypt the password.
- ``COLUMN``: the salt column must be the second column included in the query.
- ``EXTERNAL``: uses the specified algorithm to decrypt the password.


Open ID Connect Realm
~~~~~~~~~~~~~~~~~~~~~

A realm that uses the OpenID Connect (`OIDC <https://auth0.com/docs/protocols/oidc>`_) protocol to authenticate users.
:doc:`../oauth2-api/openid-connect-flow` explains the typical authentication flow when using this type of realm.

.. note::
  For agate to authenticate for an :ref:`domain-application`, its redirect URI must be set.
