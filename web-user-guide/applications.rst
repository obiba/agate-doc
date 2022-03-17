.. _applications_management:

Applications Management
=======================

An application is an external system that can use Agate as a central authentication system. Once an application is registered in agate, it can use its credentials (name and key) to connect with agate. See also :ref:`domain-application` domain documentation.

When Agate delegates the authentication to an external :ref:`oidc_realm` or when using OAuth2 service (see :ref:`oauth`), the redirect URI must be set so that Agate performs the redirect to a known application after successful authentication. Wildcard ``*`` can be used in this configured redirect URI.

The application pages are: the list of applications page and application view and edit pages.

Permissions
-----------

Users with ``agate-administrator`` role can access these pages.

Operations
----------

Add an application
~~~~~~~~~~~~~~~~~~

Creates a new application that can access agate with the defined name and key. The application name has to be unique in agate.

Edit an application
~~~~~~~~~~~~~~~~~~~

Edits an application's properties. The name can not be changed.

Delete an application
~~~~~~~~~~~~~~~~~~~~~

An application can be deleted only if there are no groups or users associated with it.
