.. Agate documentation master file, created by
   sphinx-quickstart on Mon Apr  9 11:43:58 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

OBiBa Agate Documentation
=========================

Targeted at individual studies and study consortia, `OBiBa <http://obiba.org/>`_ software stack (Opal, Mica etc.) provides a software solution for epidemiological data management, analysis and publication. While `Opal <http://www.obiba.org/pages/products/opal/>`_, the core data warehouse application, provides all the necessary tools to import, transform and describe data, `Mica <http://www.obiba.org/pages/products/mica/>`_ provides everything needed to build personalized web data portals and publish content of research activities of both studies and consortia. Based on the content defined in Mica, `Drupal <https://www.drupal.org/>`_ is the preferred platform to build your personalized web portal.

Agate is the `OBiBa <http://obiba.org/>`_'s central authentication server which intends to be easy to install and to use. Agate centralizes also some user related services such as profile management, and a notification system using emails.

.. toctree::
   :maxdepth: 1

   introduction

.. toctree::
   :maxdepth: 1
   :caption: Administrator Guide

   admin/installation
   admin/configuration
   admin/pub-configuration

.. toctree::
   :maxdepth: 1
   :caption: Web User Guide

   web-user-guide/index
   web-user-guide/users
   web-user-guide/groups
   web-user-guide/applications
   web-user-guide/tickets
   web-user-guide/realms
   web-user-guide/administration

.. toctree::
   :maxdepth: 1
   :caption: Python User Guide

   python-user-guide/index
   python-user-guide/user
   python-user-guide/group
   python-user-guide/application
   python-user-guide/other

.. toctree::
   :maxdepth: 1
   :caption: OAuth2 API

   oauth2-api/index
   oauth2-api/authorization-code-grant-flow
   oauth2-api/resource-owner-password-credentials-grant-flow
   oauth2-api/openid-connect-flow

Partners and Funders
====================

The development of this application was made possible thanks to the support of our partners and funders:

.. |mr-logo| image:: https://www.obiba.org/assets/themes/bootstrap/img/logo-maelstrom.png
 :height: 60px
 :target: https://www.maelstrom-research.org/

.. |ep-logo| image:: https://www.obiba.org/assets/themes/bootstrap/img/epigeny.png
 :height: 40px
 :target: https://www.epigeny.io/

.. |can-logo| image:: https://www.obiba.org/assets/themes/bootstrap/img/canarie.png
 :height: 60px
 :target: https://www.canarie.ca/

.. |ecc-logo| image:: https://www.obiba.org/assets/themes/bootstrap/img/eucanconnect.png
 :height: 50px
 :target: https://www.eucanconnect.eu/

+------------+------------+------------+------------+
| |mr-logo|  + |ep-logo|  + |can-logo| + |ecc-logo| +
+------------+------------+------------+------------+

Support
=======

Please visit `OBiBa support <https://www.obiba.org/pages/support/>`_ page.
