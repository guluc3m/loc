/users/<user>/toggle-follow
=====================

.. http:post:: /users/(string: username)/toggle-follow

    Toggle following status to an user.

    :param string username: Unique username of the user that (un)follow

    :<json string username: Unique username of the user to (un)follow

    :status 200: when (un)follow done. Returns ok message
    :status 401: when the user is not logged in
    :status 403: when both users are the same user
    :status 404: when user was not found

    **Example request**:

    .. sourcecode:: http

        GET /users/testuser/toggle-follow HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "username": "testuserb"
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "username": "Now following the user",
        }
