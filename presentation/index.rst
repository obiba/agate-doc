General Presentation
====================

Targeted at individual studies and study consortia, OBiBa software stack (Opal, Mica2 etc.) provides a software solution for epidemiological data management, analysis and publication. While Opal, the core data warehouse application, provides all the necessary tools to import, transform and describe data, Mica provides everything needed to build personalized web data portals and publish content of research activities of both studies and consortia. Based on the content defined in Mica, Drupal is the preferred platform to build your personalized web portal.

Agate is the OBiBa's central authentication server which intends to be easy to install and to use. Agate centralizes also some user related services such as profile management, and a notification system using emails.


Users, Groups and Applications
------------------------------

The following diagram describes the domain handled by Agate. Each entity of this domain can be edited individually in the Agate Web Application administration interface.

.. image:: images/agate-domain.png

User
~~~~

A user is described by some properties. Among these properties, the user name and email must be unique in the system: when signing in, a user can provide its name or email. The authentication is done by providing a password, which is stored in the Agate database in a digested form.

A user can belong to some groups.

A user can have access to some applications. If no application is provided, the user can only access to Agate. Otherwise, listed applications will have the user authenticated by Agate.

Group
~~~~~

A group is uniquely identified by its name. A group can be associated to one or applications.

Members of a group can have access to the applications associated to it.

Application
~~~~~~~~~~~

An application has a name and a key. Each time an external application wants to use the services of Agate, it must provide in the request its name and key. This allows Agate to check the validity of the actions to be performed and the information to be returned.

Authentication Flow
-------------------

When a user tries to sign-in an application X, this application delegates the user authentication to Agate. If successful, a ticket is created in Agate (to track user activity) and user session local to the application X is created. This local user session allows the application to not query Agate each time user authorization check is requested.

.. image:: images/agate-flow.png

Architecture, Servers and Clients
---------------------------------

The architecture of Mica is split in several servers:

* Mica server: holds the domain and controls what is to be published,
* Opal server: holds the data with their dictionary and provide statistics services,
* Agate server: user directory for data access requests management.
* Drupal server: the web portal front-end using Mica server as its source of published documents and Agate server as its user directory.

Mica, Opal and Agate are applications developed by OBiba. OBiBa also provides extensions for the Drupal application. Each of these OBiBa servers expose web services to allow easy interconnection. The Mica web portal is the final application which leverages each server specific domain and functionalities in one.

The following diagram shows how these servers are linked together:

.. image:: images/mica-architecture.png


Agate Server
~~~~~~~~~~~~

Agate application is used for:

* having a user directory shared between OBiBa's applications,
* having centralized services such as profile management and email notifications.

Mica Server
~~~~~~~~~~~

Mica application is used for:

* defining and publishing network, study and dataset catalogues,
* search for variables.

Installation and configuration guides can be found in the section Mica Server Administrator Guide.

Editors and reviewers of the Mica web portal content can access to the web interface of this server as described in the Mica Web Application User Guide.

Mica server is a client of Opal and Agate servers.

Opal Server
~~~~~~~~~~~

Opal application is used for:

* defining data dictionaries (variables),
* storing data,
* providing data summary statistics.

Opal offers well established security controls, allowing to NOT expose individual-level data. Note also that the Opal server is only accessed by the Mica server, reducing the risk of data compromisation from a malicious end user.

Installation and configuration guides can be found in the Opal Server Administrator Guide.

Mica expects at least one Opal server when some datasets are defined. Additional Opal servers can also be identified to access to distributed datasets.

Drupal Server
~~~~~~~~~~~~~

Drupal is a content management system, i.e. an application allowing to build fully customizable web portals. Drupal can be extended by modules and themes: Mica and Agate modules have been developed to access to the services of these servers. Drupal server is therefore a client of Mica and Agate servers.

Installation and configuration guides about Drupal as a Mica client can be found in the Mica Drupal Client User Guide documentation.
