/profile/edit
=============

.. http:get:: /profile/edit

    Obtain the user information that can be edited.

    :<json string token: JWT token of the active user session

    :>json string name: real name of the current user
    :>json string email: email address used to communicate with the user

    :status 200: when user is logged in and its user record exists
    :status 401: when the user is not logged in
    :status 404: when user record has been removed

    **Example request**:

    .. sourcecode:: http

        GET /profile/edit HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "token": "JWT_TOKEN"
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "name": "Current User",
            "email": "user@example.com"
        }


.. http:post:: /profile/edit

    Update the information stored for the current user

    :<json string token: JWT token of the active user session
    :<json string name: real name of the current user (not required)
    :<json string email: email address used to communicate with the user

    :status 200: when user is logged in and record exists; returns updated info
    :status 401: when the user is not logged in
    :status 404: when user record has been removed
    :status 409: when the provided email is already in use

    **Example request**:

    .. sourcecode:: http

        GET /profile/edit HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "token": "JWT_TOKEN",
            "name": "New user",
            "email": "newmail@example.com"
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "name": "New User",
            "email": "newmail@example.com"
        }
