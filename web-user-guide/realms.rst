Realms Management
=================

Authentication Delegation
-------------------------

Agate is able to delegate authentication to alternate identity provider systems. Note that even if the authentication happens in this third party application, the user still need to have a profile declared in Agate. The sign-up process extracts the user information, if some are available, to assist with the creation of this profile, but afterwards only the authentication service is used.

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

A realm that uses the OpenID Connect (`OIDC <https://openid.net/connect/>`_) protocol to authenticate users.
:doc:`../oauth2-api/openid-connect-flow` explains the typical authentication flow when using this type of realm.

.. note::
  For agate to authenticate for an :ref:`domain-application`, its redirect URI must be set.
