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

The **Fallback notification templates** property is the templates folder from which the notification templates missing in the application's own folder are taken (for instance the ``mica`` folder for a Mica server registered with another name). "None" means no fallback.

Notification templates
~~~~~~~~~~~~~~~~~~~~~~

The application page lists the templates of the notification emails that the application can request Agate to send to its users. These templates are located in the **notifications/<application ID>** folder. Each template is presented with its state:

* **Default**, the template provided with Agate,
* **Custom**, a template that was added,
* **Overridden**, a custom template replacing the default one,
* **Inherited from <folder>**, a template of the fallback folder, not defined in the application's folder.

The template editor offers:

* **Edit**: the `FreeMarker <https://freemarker.apache.org/>`_ template source, with syntax highlighting. The template is validated before being saved. Saving an inherited template creates a copy of it in the application's folder.
* **Preview**: the HTML rendering of the template being edited (not necessarily saved), with the current user as the recipient, in the selected language (one of the configured languages; defaults to the template name suffix, if any). The statements using variables that are provided by the application when requesting the notification are skipped.

Removing a custom template restores the default or inherited one, if any. The edited templates are saved in the **AGATE_HOME/conf/templates/notifications/<application ID>** folder.

Delete an application
~~~~~~~~~~~~~~~~~~~~~

An application can be deleted only if there are no groups or users associated with it.
