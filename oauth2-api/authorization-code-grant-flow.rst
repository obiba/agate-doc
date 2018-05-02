Authorization Code Grant Flow
=============================

Summary
-------

When a client application wants access to the resources of a resource owner, hosted on a resource server, the client application must first obtain an `authorization code grant <https://tools.ietf.org/html/rfc6749#section-4.1>`_ from the authorization server (Agate). The following explains how such a grant is obtained.

Step 1. Authorization
---------------------

Request
~~~~~~~

The client application must redirect the user to the Agate authorization page which is:

.. code-block:: rest

  GET https://agate.example.org/ws/oauth2/authorize?<PARAMETERS>

The following values should/could be passed as parameters:

=================== ===================
Parameter           Description
=================== ===================
``client_id``	      Client application that will be granted the authorization (required).
``response_type``   The expected value is: code (required).
``scope``           Space separated application names (required).
``redirect_uri``    URL to redirect back to (optional, if not specified default client application redirect URI will be used).
``state``           Unique string to be passed back upon completion (optional, recommended).
=================== ===================

Agate will redirect the "user-agent" (usually a web browser) to a web page where the resource owner can grant or deny the requested authorizations.

Response
~~~~~~~~

If the resource owner accepts to grant the requested authorizations to the client application, then the response will consist of a redirect to the provided redirect_uri with the following request parameters:

=================== ===================
Parameter           Description
=================== ===================
``code``	          The authorization code.
``state``	          The state parameter value that was provided in the request (if any).
``expires_in``	    Information about the expiration time (in seconds) before the authorization expires.
=================== ===================

The redirect request will then look like:

.. code-block:: rest

  GET https://client.example.org/redirect?code=AUTHORIZATION_CODE&state=STATE&expires_in=7775999

From then, it is the responsibility of the client application to response to this request with a redirect to the relevant client application page.

Errors
~~~~~~

The following response errors can be encountered during this step.

.. code-block:: rest

  GET https://client.example.org/redirect?error=ERROR_CODE&error_description=ERROR_MESSAGE


==================================== ===================
Parameter                            Description
==================================== ===================
``access_denied``                    When the user refuses to grant the requested authorization.
``invalid_scope``                    The requested scope is not one of the declared resource application scopes.
``missing_application_redirect_uri`` The client application does not have a default redirect URI. This is a client application definition issue.
``invalid_redirect_uri``             The provided redirect URI does not starts with the client application's default redirect URI.
``server_error``                     Other errors.
==================================== ===================

Step 2. Access Token Issuing
----------------------------

Request
~~~~~~~

The REST endpoint to be used is:

.. code-block:: rest

  POST https://agate.example.org/ws/oauth2/token

The form parameters to be sent within the body of the request are:

client_id	Client application name (required).
client_secret	Client application secret key (required).
grant_type	The expected value is: authorization_code (required).
code	The authorization code from the Step 1 (required).
redirect_uri	Must match the originally submitted URI (if one was sent).

Response
~~~~~~~~

The response is a JSON object with the following properties:

=================== ===================
Property            Description
=================== ===================
``access_token``	  The access token. Agate provides signed tokens that implement the JSON Web Token specification.
``token_type``	    What you can do with this token; in the case of Agate the value for this property is bearer.
``expires_in``      Information about the expiration time (in seconds) before the token expires.
=================== ===================


An example of response would be:

.. code-block:: json

  {
      "access_token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJlZGl0b3IiLCJpc3MiOiJhZ2F0ZTo1NmZjMzg0MmNjZjJjMWM3ZWM1YzVkMTQiLCJpYXQiOjE0NTk0NTg0NTgsImV4cCI6MTQ1OTQ4NzI1OCwianRpIjoiNTZmZDkxOWFjY2YyYzFjN2VjNWM1ZDE2IiwiYXVkIjpbIm1pY2EiLCJ0b3RvIl0sImNvbnRleHQiOnsic2NvcGVzIjpbIm1pY2EiXSwidXNlciI6eyJuYW1lIjoiSnVsaWUiLCJncm91cHMiOlsibWljYS1lZGl0b3IiXSwiZmlyc3RfbmFtZSI6Ikp1bGllIn19fQ.PqlLSZegdPLM2byp0jsgWV-XM3Xed8DP4I03kbUUEeo",
      "token_type": "bearer",
      "expires_in": 28799
  }

Being a JSON Web Token (JWT), the access token can be decoded. There are three parts in a JWT: the header, the payload and the signature. This could give for example:

.. code-block:: text

  {
      "alg": "HS256"
  }
  .
  {
      "sub": "editor",
      "iss": "agate:56fc3842ccf2c1c7ec5c5d14",
      "iat": 1459458458,
      "exp": 1459487258,
      "jti": "56fd919accf2c1c7ec5c5d16",
      "aud": [
          "mica",
          "client_app"
      ],
      "context": {
          "scopes": [
              "mica"
          ],
          "user": {
              "name": "Julie LaTendresse",
              "groups": [
                  "mica-editor"
              ],
              "first_name": "Julie",
              "last_name": "Latendresse"
          }
      }
  }
  .
  [signature]

The JWT payload contains some basic details on the subject (in addition to the standard claims). These are available in the context object (which is a claim specific to Agate). The properties of the context are:

=================== ===================
Property            Description
=================== ===================
``user.name``	      The user full name for display.
``user.first_name`` The user first name (if any).
``user.last_name``  The user last name (if any).
``user.groups``     The user groups.
``scopes``          Reminder of the scopes associated to the authorization code grant.
=================== ===================

Note that this step can be repeated as many times as necessary, using the same authorization code that was granted at step 1.

Errors
~~~~~~

When an error is encountered during this step, the JSON object returned contains the description of the error, for example:

.. code-block:: json

  {
      "error_description":"Authorization with code '3b1d664fb09407972d4c212081789c6f' does not exist",
      "error":"NoSuchAuthorizationException"
  }

Step 3. Resource Access
-----------------------

The client application will use the access token as a bearer of resource owner identity to get the resource from the resource server. How the access token should be passed to the resource application is out of the concern of Agate.

Most common practice (this is the case for Opal and Mica) is that the access token is placed in the headers of the HTTP request issued by the client application on the resource server. This can be expressed as a `curl <https://curl.haxx.se/>`_ command:

.. code-block:: bash

  curl -X GET --header "Authorization: Bearer ACCESS_TOKEN" http://resource.example.org/some/path

Step 4. Access Token Validation
-------------------------------

The resource server has received an access token from a client application. Although the access token delivered by Agate is a JWT that contains in its payload all the basic information (subject identification, authorized scopes), it is the responsibility of the resource application to validate this token.

This can be achieved by requesting the REST end point:

.. code-block:: rest

  GET https://agate.example.org/ws/ticket/ACCESS_TOKEN/_validate

Note that the resource application must identifies itself in this request. This can be expressed as a curl command:

.. code-block:: bash

  curl -X GET --header "X-App-Auth: Basic `echo -n "APPLICATION_NAME:APPLICATION_KEY" | base64`" https://agate.example.org/ws/ticket/ACCESS_TOKEN/_validate

The expected response code is *200* (*OK*), without a response body.

Possible validation errors are:

* application could not be identified,
* access token signature verification has failed,
* access token issuer is not the current Agate instance,
* application is not part of the audience of the access token,
* access token has expired,
* user is not active any more.
