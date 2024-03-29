Realms Management
=================

Authentication Delegation
-------------------------

Agate is able to delegate authentication to alternate identity provider systems. Note that even if the authentication happens in this third party application, the user still need to have a profile declared in Agate. The sign-up process extracts the user information, if some are available, to assist with the creation of this profile, but afterwards only the authentication service is used.

Realm Types
-----------

.. _oidc_realm:

Open ID Connect Realm
~~~~~~~~~~~~~~~~~~~~~

A realm that uses the OpenID Connect (`OIDC <https://openid.net/connect/>`_) protocol to authenticate users.
:doc:`../oauth2-api/openid-connect-flow` explains the typical authentication flow when using this type of realm.

To register Agate as a client of the OIDC provider it will be necessary to provide its callback URL which is: ``https://agate.example.org/auth/callback/``.

.. note::
  For Agate to authenticate for an :ref:`domain-application`, the redirect URI of the Application must be set (see :ref:`applications_management`).

An example of well known open source ID provider that can be declared as an OIDC realm is `Keycloak <https://www.keycloak.org/>`_. Keycloak has also a strong user federation feature, which we recommend to use instead of using the following other realm types (LDAP etc.).

There following fields are required:

* An ID provider must be identified by a *Name*,
* The Agate application has been registered in the this provider: these are the *Client ID* and *Client Secret* fields.
* The *Discovery URI* must follow the `OpenID Connect configuration discovery specifications <https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderConfig>`_.

The optional fields are:

* *Title* is a human-readable name that will be displayed in the provider's signin button in the login page. If missing, the name of the ID provider will be used.
* *Groups* are the group that are to be automatically applied to any users signing in through this ID provider.
* *Account Login* address allows the user to go to it's personal profile page in the ID provider interface (to chenge its password for instance) from the Opal login page.
* *Scope* is the scope value(s) to be sent to the ID provider to initiate the OpenID Connect dialog. This is provider dependent but usually ``openid`` is enough.
* *User Information Mapping* specify which field values of the `UserInfo <https://openid.net/specs/openid-connect-core-1_0.html#UserInfo>`_ object will be applied to the new Agate user.
* *Groups by Claim* is an optional field name in the `UserInfo <https://openid.net/specs/openid-connect-core-1_0.html#UserInfo>`_ object (that is returned by the ID provider) that contains the group names to which the user belongs. These will be automatically applied to the user's profile. Such field is `not one of the standard claims <https://openid.net/specs/openid-connect-core-1_0.html#StandardClaims>`_ and needs to be explicitly set. The expected value type associated to this claim is either an array of strings, or a string which group names are separated by spaces (or commas).
* *Groups by JS* is an optional Javascript code chunk that will process the `UserInfo <https://openid.net/specs/openid-connect-core-1_0.html#UserInfo>`_ object to extract a group name or an array of group names to which the Agate user will belong.
* *Public URL* is the public base URL of the server that will be used when sending notification emails and building an OpenID Connect callback URL.

Note that the groups mapping (by claim or JS) is executed at each sign in. Then if the user was associated to a new group in the OIDC provider, this group will be automatically applied to the corresponding Agate's user as well. If the group does not exist yet, it will be created (without associated application). Removing an OIDC user from a group does not remove the Agate user from the group with same name.

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
