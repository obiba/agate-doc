Resource Owner Password Credentials Grant Flow
==============================================

Summary
-------

The  `resource owner password credentials grant <https://tools.ietf.org/html/rfc6749#section-4.3>`_ is suitable for clients capable of obtaining the resource owner's credentials (username and password, typically using an interactive form). This implies that the resource owner has a trust relationship with the client application, such as the device operating system or a highly privileged application.

Agate's implementation of this flow is very limited. The access token obtained with this flow does not provide authorization to access the resource applications. This flow's main use case is to authenticate the resource owner.

Access Token Issuing
--------------------

Request
~~~~~~~

The REST end point to be used is:

.. code-block:: rest

  POST https://agate.example.org/ws/oauth2/token

The form parameters to be sent within the body of the request are:

================= ===================
Parameter                            Description
================= ===================
``client_id``	    Client application name (required).
``client_secret``	Client application secret key (required).
``grant_type``    The expected value is: password (required).
``username``      The user name.
``password``      The user password.
================= ===================

Response
~~~~~~~~
The response is a JSON object with the following properties:

================ ===================
Property         Description
================ ===================
``access_token`` The access token. Agate provides signed tokens that implement the JSON Web Token specification.
``token_type``   What you can do with this token; in the case of Agate the value for this property is bearer.
``expires_in``   Information about the expiration time (in seconds) before the token expires.
================ ===================

An example of response would be:

.. code-block:: json

  {
      "access_token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJlZGl0b3IiLCJpc3MiOiJhZ2F0ZTo1NmZjMzg0MmNjZjJjMWM3ZWM1YzVkMTQiLCJpYXQiOjE0NTk0NTg0NTgsImV4cCI6MTQ1OTQ4NzI1OCwianRpIjoiNTZmZDkxOWFjY2YyYzFjN2VjNWM1ZDE2IiwiYXVkIjpbIm1pY2EiLCJ0b3RvIl0sImNvbnRleHQiOnsic2NvcGVzIjpbIm1pY2EiXSwidXNlciI6eyJuYW1lIjoiSnVsaWUiLCJncm91cHMiOlsibWljYS1lZGl0b3IiXSwiZmlyc3RfbmFtZSI6Ikp1bGllIn19fQ.PqlLSZegdPLM2byp0jsgWV-XM3Xed8DP4I03kbUUEeo",
      "token_type": "bearer",
      "expires_in": 28799
  }

Being a `JSON Web Token <https://jwt.io/>`_ (JWT), the access token can be decoded. There are three parts in a JWT: the header, the payload and the signature. This could give for example:

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
          "client_app"
      ],
      "context": {
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

The JWT payload contains some basic details on the subject (in addition to the `standard claims <https://tools.ietf.org/html/rfc7519#section-4.1>`_). These are available in the context object (which is a claim specific to Agate). The properties of the context are:

=================== ===================
Property            Description
=================== ===================
``user.name``       The user full name for display.
``user.first_name`` The user first name (if any).
``user.last_name``  The user last name (if any).
``user.groups``     The user groups.
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
