/users/<user>
=============

.. http:get:: /users/(string: username)

    Show user profile.

    :param string username: the username for which to obtain information

    :>json string username: the username of the obtained user
    :>json string name: real name of the obtained user
    :>json int points: total score of the obtained user

    :status 200: when user record was found. Returns user information
    :status 404: when user record was not found or was deleted

    **Example request**:

    .. sourcecode:: http

        GET /users/testuser HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "username": "testuser",
            "name": "Test User",
            "points": 42
        }
