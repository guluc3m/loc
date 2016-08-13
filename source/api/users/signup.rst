/signup
=======

.. http:post:: /signup

    Create a new user record in the server.

    Also performs login automatically.

    :<json string username: the username for the new user (**unique**)
    :<json string email: the email for the new user (**unique**)
    :<json string password: password for the new user in the login

    :>json string token: JWT token for the user session

    :status 201: when user is created correctly; returns the JWT token
    :status 409: when an user with the specified username/email already exists

    **Example request**:

    .. sourcecode:: http

        POST /signup HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "username": "test_user",
            "email": "my_email@example.com",
            "password": "mypassword"
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 201 OK
        Content-Type: application/json

        {
            "token": "JWT_TOKEN"
        }
