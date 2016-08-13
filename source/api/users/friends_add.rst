/profile/friends/add
====================

.. http:post:: /profile/friends/add

    Add a new friend to the user's profile.

    This should create a friend request and notify the added user about it so
    that they can accept/reject the invitation.

    Friend associations work 2-way.

    :<json string token: JWT token of the active user session
    :<json string username: the username of the friend to add

    :status 200: when user is logged in
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
            "username": "frienda"
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json
