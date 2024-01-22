Administration
==============

The Administration section is available to users with the role ``agate-administrator``. This menu gives access to server configuration and status.

Properties
----------

The following general configuration properties can be modified:

========================================== ==========================================
Property                                   Description
========================================== ==========================================
Name	                                     The name of the organization using this instance of Agate server. It will be used when sending notification emails.
Public URL                                 Public base URL of the server. It will be used when sending notification emails and in the OAuth2 settings.
Portal URL                                 The organization main portal, to go back to the main site form the Agate's public pages.
Domain                                     The session cookie domain, required for single sign-on to operate.
Short term timeout                         Ticket expiration timeout in hours.
Long term timeout                          Ticket expiration timeout in hours when "remember me" option is selected.
Inactive timeout                           User account expiration timeout in days.
Sign up enabled                            Whether a user can self register from Agate public pages. This does not prevent from Mica to expose the sign-up feature.
Sign up form offers to choose the username User name will be extracted from user email.
========================================== ==========================================

Encryption Keys
---------------

This section presents the tool related to the encryption through HTTPS of transactions between Agate and its clients by means of a trusted or a self-signed certificate.

.. note::

  In the instruction below, when you are told to cut and paste the content of the certificate, private key or of an .pem file, make sure that you copy all content, that is including the lines containing ``-----BEGIN XXXXXXXX-----`` and ``-----END XXXXXXXX-----``.

Create a (self-signed) certificate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Useful when in testing phase, not recommended in production.

1. Click on the *Add Keys* drop-down.
2. Select *Create*.
3. Fill in the form and click on Save.
4. Click on the Download Certificate button under the section title Encryption Keys.

Your certificate (.pem file) should automatically be downloaded on your computer.

Import a certificate
~~~~~~~~~~~~~~~~~~~~

It is recommended to use a valid key pair in production.

1. Click on the *Add Keys* drop-down
2. Select *Import*. Here you may use (1) certificate and (2) private key that you created using third party software e.g., `OpenSSL <https://www.openssl.org/>`_. Note that both the certificate and the private key must be in PEM format.
3. Save.
4. Finally, in order for the changes to be taken in account you need to restart Agate server.

User Attributes
---------------

Additional user attributes can be declared. They will appear in the user form (including sign-up).
