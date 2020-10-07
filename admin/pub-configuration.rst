Public Pages Configuration
==========================

Starting from Agate 2.0, the administration user interface is distinct from the public pages, i.e. pages that are to be accessed by regular users. These pages are based on templates that can be customized, extended or overridden. The template engine that is used is `FreeMarker <https://freemarker.apache.org/>`_ which has a clean and powerful syntax.

Page Templates
--------------

Main Pages
~~~~~~~~~~

The main public pages are:

=================== ==================
Page                Description
=================== ==================
``index``           The home page
``profile``         The user profile page for updating personal information and password
``signin``          The login page
``signup``          The user registration page
``signup-with``     The user registration page, with form pre-filled with personal information extracted from a OpenID Connect server
``confirm``         The page to confirm user's registration (and validate email) and set the user password
``forgot-password`` The page to ask for password reset
``reset-password``  The page to update the password after a reset was triggered
``just-registered`` The welcome page after a user has registered
=================== ==================

The `templates structure <https://github.com/obiba/agate/blob/master/agate-webapp/src/main/resources/_templates/>`_ is organized in a way that it should not be necessary to override these pages definitions. Instead of that, it is recommended to change/extend the theme/style as described in this guide.

Some template variables (date formats, branding, favicon etc.) are also defined in `libs/settings.ftl <https://github.com/obiba/agate/blob/master/agate-webapp/src/main/resources/_templates/libs/settings.ftl>`_ and can be altered in the file **models/settings.ftl** that would be added in your configuration folder as follows:

.. code-block:: bash

  AGATE_HOME
  └── conf
      └── templates
          └── models
              └── settings.ftl

**General settings**

.. list-table::
   :widths: 10 90
   :header-rows: 1

   * - Variable
     - Description
   * - ``datetimeFormat``
     - The format in which the date-time values should be rendered.
   * - ``date``
     - The format in which the date values should be rendered.
   * - ``faviconPath``
     - The location of the favicon, to be modified to match your own.
   * - ``brandImageSrc``
     - The location of your organization's logo.
   * - ``brandImageClass``
     - CSS classes to apply to the logo.
   * - ``brandTextEnabled``
     - Logical to show/hide a text aside of the logo.
   * - ``brandTextClass``
     - CSS classes to apply to the text aside of the logo.
   * - ``adminLTEPath``
     - The location of the `AdminLTE <https://adminlte.io/>`_ theme if this one has been modified (see the **Theme** section in this documentation).

**Home page settings**

.. list-table::
  :widths: 10 90
  :header-rows: 1

  * - Variable
    - Description
  * - ``portalLink``
    - The link applied to the logo. Default is the data portal (as specified in the Administration > General section), but it could also be the organization's main portal.

**User Profile page settings**

.. list-table::
   :widths: 10 90
   :header-rows: 1

   * - Variable
     - Description
   * - ``showProfileRole``
     - Logical to show/hide the role to which the user belongs.
   * - ``showProfileGroups``
     - Logical to show/hide the groups to which the user belongs.
   * - ``showProfileApplications``
     - Logical to show/hide the applications in which the user can sign.


Adding Pages
~~~~~~~~~~~~

It is possible to add new pages, for providing additional information or guidance to the regular user. This can be done as follows:

* Install a new page templates
* Add a new menu entry

**1. Install custom page template**

The new template page is to be declared in the configuration folder:

.. code-block:: bash

  AGATE_HOME
  └── conf
      └── templates
          └── custom.ftl

You can check at the provided templates to make your template fit in the site theme and structure. The `profile page template <https://github.com/obiba/agate/blob/master/agate-webapp/src/main/resources/_templates/profile.ftl>`_ could be a good starting point.

`FreeMarker <https://freemarker.apache.org/>`_ will look at its context to resolve variable values. For a custom page the objects available in the context are:

================ ================
Object           Description
================ ================
``config``       The Agate configuration
``user``         The user object (if user is logged in)
``query``        The URL query parameters as a map of strings
================ ================

This custom template page can load any CSS or JS file that might be useful. These files can be served directly by adding them as follows (there are no restrictions regarding the naming and the structure of these files, as soon as they are located in the **static** folder):

.. code-block:: bash

  AGATE_HOME
  └── conf
      └── static
          ├── custom.css
          └── custom.js

The URL of this custom page will be for instance: ``https://agate.example.org/page/custom``.

**2. Custom menu entry**

To link to a custom page (or an external page), some templates can be defined to extend the default menus: left menu can be extended on its right and right menu can be extended on its left. The corresponding templates are:

.. code-block:: bash

  AGATE_HOME
  └── conf
      └── templates
          └── models
              ├── navbar-menus-left.ftl
              └── navbar-menus-right.ftl

Check at the default `left <https://github.com/obiba/agate/blob/master/agate-webapp/src/main/resources/_templates/libs/navbar-menus-left.ftl>`_ and `right <https://github.com/obiba/agate/blob/master/agate-webapp/src/main/resources/_templates/libs/navbar-menus-right.ftl>`_ menus implementation as a reference.

Theme and Style
---------------

Theme
~~~~~

The default theme is the one provided by the excellent `AdminLTE <https://adminlte.io/>`_ framework. It is based on `Bootstrap <https://getbootstrap.com/>`_ and `JQuery <https://jquery.com/>`_. In order to overwrite this default theme, the procedure is the following:

* Build a custom AdminLTE distribution
* Install this custom distribution
* Change the template settings so that pages refer to this custom distribution instead of the default one

**1. Build custom AdminLTE**

This requires some knowledge in CSS development in a Node.js environment:

* Download `AdminLTE source <https://github.com/ColorlibHQ/AdminLTE>`_ (source code or a released version)
* Reconfigure `Sass <https://sass-lang.com/>`_ variables
* Rebuild AdminLTE (see instructions in the README file, contributions section)

**2. Install custom AdminLTE**

The objective is to have the web server to serve this new set of stylesheet and javascript files. This is achieved by creating the folder **AGATE_HOME/conf/static** and copying the AdminLTE custom distribution in that folder. Not all the AdminLTE are needed, only the **dist** and **plugins** ones. The folder tree will look like:

.. code-block:: bash

  AGATE_HOME
  └── conf
      └── static
          └── admin-lte
              ├── dist
              └── plugins


**3. Template settings**

Now that the custom AdminLTE distribution is installed in the web server environment, this new location must be declared in the page templates. The default templates settings are defined in the `libs/settings.ftl <https://github.com/obiba/agate/blob/master/agate-webapp/src/main/resources/_templates/libs/settings.ftl>`_ template file. See the **adminLTEPath** variable. This variable can be altered by defining a custom **settings.ftl** file as follows:

.. code-block:: bash

  AGATE_HOME
  └── conf
      └── templates
          └── models
              └── settings.ftl

In this custom **settings.ftl** file the new AdminLTE distribution location will be declared:

.. code-block:: xml

  <#assign adminLTEPath = "/admin-lte"/>

Style
~~~~~

As an alternative to theming, it is also possible to alter the style of the pages by loading your own stylesheet and tweaking the pages' layout using javascript (and `JQuery <https://jquery.com/>`_). The procedure is the following:

* Install custom CSS and/or JS files
* Custom the templates to include these new CSS and/or JS assets

**1. Install custom CSS/JS**

The objective is to have the web server to serve this new set of stylesheet and javascript files. This is achieved by creating the folder **AGATE_HOME/conf/static** and copying any CSS/JS files that will be included in the template pages. The folder tree will look like:

.. code-block:: bash

  AGATE_HOME
  └── conf
      └── static
          ├── custom.css
          └── custom.js

**2. Custom templates**

For the CSS files, the **models/head.ftl** template allows to extend the HTML pages "head" tag content with custom content. For the JS files, the **models/scripts.ftl** template allows to extend the HTML pages "script" tags. The folder tree will look like:

.. code-block:: bash

  AGATE_HOME
  └── conf
      └── templates
          └── models
              ├── head.ftl
              └── scripts.ftl

Where the **head.ftl** template will be:

.. code-block:: xml

  <link rel="stylesheet" href="/custom.css"/>

And the **scripts.ftl** template will be:

.. code-block:: xml

  <script src="/custom.js"/>


Translations
------------

The translations are performed in the following order, for a given ``locale``:

1. check for the message key in the messages_<locale>.properties (at different locations)
2. check for the message key in the <locale> JSON object as defined the **Administration > Translations** section of the administration interface

For the messages_* properties, the translations can be added/overridden as follows:

.. code-block:: bash

  AGATE_HOME
  └── conf
      └── translations
          ├── notifications
          │   ├── messages_fr.properties
          │   └── messages_en.properties
          ├── messages_fr.properties
          └── messages_en.properties

Note that the notification emails translations are located at a different place than the ones for the public pages. Note also that you can declare only the messages_* properties files that are relevant (language and public pages vs. notification emails) and the content of these files can contain only the translation keys that you want to override.
