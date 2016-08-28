/profile/friends/accept/<token>
===============================

.. http:post:: /profile/friends/accept/(string: token)

    Accept a friend request.

    A new Friend record is added for both users.

    :param string username: the username for which to obtain information

    :<json string token: JWT token of the active user session

    :status 200: when user is logged in, request is accepted
    :status 401: when the user is not logged in
    :status 404: when user record has been removed or does not exist

    **Example request**:

    .. sourcecode:: http

        GET /profile/friends HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "token": "JWT_TOKEN",
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

