OpenID Connect Flow
===================

Summary
-------

OpenID connect is an extension on top of OAuth2, so the authorization and token endpoints are the same as described in :doc:`index`. Currently the `OpenID Connect <http://openid.net/specs/openid-connect-core-1_0.html>`_ implementation in Agate only supports the authorization code flow.

Agate implements the OpenID Connect configuration discovery specification (scopes, endpoints, algos etc.). The discovery request would look like:

.. code-block:: rest

  GET https://agate.example.org/.well-known/openid-configuration

Step 1. Authorization
---------------------

This first step is the same as in the one in the Authorization Code Grant Flow: see authorization request and response. The scope to be requested must contain at least openid in addition to more specific scopes. Currently the only supported scopes are: email and profile.

============ ===================
Scope        Description
============ ===================
``openid``   User name (required).
``email``	   User email address and whether this email was verified (optional).
``profile``	 User first and last names (optional) and groups.
``phone``	   User phone (not supported).
``address``	 User address (not supported).
============ ===================

An example of an OpenID connect authorization request will then look like:

.. code-block:: rest

  GET https://agate.example.org/ws/oauth2/authorize?client_id=xxx&response_type=code&scope=openid+email+profile

Step 2. ID Token Issuing
------------------------

This second step is similar to the access token issuing. When the authorization includes the openid scope, the response will contain an additional id_token in JWT format. An example of the reponse is:

.. code-block:: json

  {
    "access_token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VyMSIsImlzcyI6ImFnYXRlOjU3MDUyMjA2ZTRiMGRlNDNlYzE5NzM2YiIsImlhdCI6MTQ2MDA0MTU4NCwiZXhwIjoxNDYwMDcwMzg0LCJqdGkiOiI1NzA2Nzc3MGU0YjBmZjM3ODJkYmQ2MjIiLCJhdWQiOlsiZHJ1cGFsIl0sImNvbnRleHQiOnsic2NvcGVzIjpbIm9wZW5pZCJdLCJ1c2VyIjp7Im5hbWUiOiJKb2hubnkgQi4gR29vZCIsImxhc3RfbmFtZSI6Ikdvb2QiLCJncm91cHMiOlsibWljYS11c2VyIl0sImZpcnN0X25hbWUiOiJKb2hubnkgQi4ifX19.7SblBktnvXaoBFL61Rx_jb6PXXYPr4TFMlyi4ZYP5xE",
    "id_token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1c2VyMSIsImlzcyI6ImFnYXRlOjU3MDUyMjA2ZTRiMGRlNDNlYzE5NzM2YiIsImdpdmVuX25hbWUiOiJKb2hubnkgQi4iLCJmYW1pbHlfbmFtZSI6Ikdvb2QiLCJuYW1lIjoiSm9obm55IEIuIEdvb2QiLCJlbWFpbCI6ImpvaG55Lmdvb2RAZXhhbXBsZS5jb20iLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImlhdCI6MTQ1OTk3Mzc1OCwiZXhwIjoxNDY3NzQ5NzU4LCJhdWQiOiJkcnVwYWwifQ.1IqjodUNGZ8pKnlxmjzR0XcDgs8Hnl-ufeFsSNH3qaA",
    "expires_in": 28799,
    "token_type": "bearer"
  }

The id_token represents the following structure (when using scope=openid email profile):

.. code-block:: text

  {
      "alg": "HS256"
  }
  .
  {
      "sub": "user1",
      "iss": "agate:57052206e4b0de43ec19736b",
      "given_name": "Johnny B.",
      "family_name": "Good",
      "name": "Johnny B. Good",
      "email": "johny.good@example.com",
      "email_verified": false,
      "iat": 1459973758,
      "exp": 1467749758,
      "aud": "drupal"
  }
  .
  [signature]

Step 3. ID Resource Access
--------------------------

In addition to the id_token included in the access token response, the user information can be retrieved from the `UserInfo <http://openid.net/specs/openid-connect-core-1_0.html#UserInfo>`_ end point. This step is similar to the resource access one (the resource is then the user information).

An example of ID resource request is:

.. code-block:: bash

  curl -X GET --header "Authorization: Bearer ACCESS_TOKEN" https://agate.example.org/ws/oauth2/userinfo

The response is in JSON format and contains the user profile claims. An example of a response is:

.. code-block:: json

  {
    "family_name": "Good",
    "sub": "user1",
    "iss": "agate:57052206e4b0de43ec19736b",
    "email_verified": false,
    "given_name": "Johnny B.",
    "email": "johny.good@example.com",
    "name": "Johnny B. Good"
  }
