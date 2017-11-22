/login
======

.. http:post:: /login

    Obtain JWT token from the server required in the rest of the resources.

    :<json string username: the username for which to start a session
    :<json string password: password to use in the login
    :<json string remember_me: whether or not the session should remain active

    :>json string token: JWT token for the user session

    :status 200: when credentials are valid; returns the JWT token
    :status 401: when credentials are not valid

    **Example request**:

    .. sourcecode:: http

        POST /login HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "username": "test_user",
            "password": "mypassword",
            "remember_me": false
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "token": "JWT_TOKEN"
        }
