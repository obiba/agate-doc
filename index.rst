.. Agate documentation master file, created by
   sphinx-quickstart on Mon Apr  9 11:43:58 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

OBiBa Agate Documentation
=========================

Targeted at individual studies and study consortia, `OBiBa <http://obiba.org/>`_ software stack (Opal, Mica2 etc.) provides a software solution for epidemiological data management, analysis and publication. While `Opal <http://www.obiba.org/pages/products/opal/>`_, the core data warehouse application, provides all the necessary tools to import, transform and describe data, `Mica <http://www.obiba.org/pages/products/mica/>`_ provides everything needed to build personalized web data portals and publish content of research activities of both studies and consortia. Based on the content defined in Mica, `Drupal <https://www.drupal.org/>`_ is the preferred platform to build your personalized web portal.

Agate is the `OBiBa <http://obiba.org/>`_'s central authentication server which intends to be easy to install and to use. Agate centralizes also some user related services such as profile management, and a notification system using emails.

.. toctree::
   :maxdepth: 1

   introduction

.. toctree::
   :maxdepth: 1
   :caption: Administrator Guide

   installation
   configuration

.. toctree::
   :maxdepth: 1
   :caption: Web User Guide

   web-user-guide/index
   web-user-guide/users
   web-user-guide/groups
   web-user-guide/applications
   web-user-guide/tickets
   web-user-guide/administration

.. toctree::
   :maxdepth: 1
   :caption: Python User Guide

   python-user-guide/index
   python-user-guide/add-user
   python-user-guide/delete-user
   python-user-guide/add-group
   python-user-guide/delete-group
   python-user-guide/add-application
   python-user-guide/delete-application
   python-user-guide/rest

.. toctree::
   :maxdepth: 1
   :caption: OAuth2 API

   oauth2-api/index
   oauth2-api/authorization-code-grant-flow
   oauth2-api/resource-owner-password-credentials-grant-flow
   oauth2-api/openid-connect-flow
