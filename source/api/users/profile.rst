/profile
========

.. http:get:: /profile

    Obtain current logged in user details.

    :<json string token: JWT token of the active user session

    :>json string username: the username of the current user
    :>json string name: real name of the current user
    :>json int points: total score of the current user

    :status 200: when user is logged in and its user record exists
    :status 404: when user record has been removed

    **Example request**:

    .. sourcecode:: http

        GET /profile HTTP/1.1
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
            "username": "currentuser",
            "name": "Current User",
            "points": 42
        }
