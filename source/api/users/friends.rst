/profile/friends
================

.. http:get:: /profile/friends

    Obtain current logged in user's friends.

    :<json string token: JWT token of the active user session

    :>json string username: the username of the current user
    :>json string image: URL to their profile image

    :status 200: when user is logged in; returns list of friends (may be empty)
    :status 401: when the user is not logged in
    :status 404: when user record has been removed

    **Example request**:

    .. sourcecode:: http

        GET /profile/friends HTTP/1.1
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

        [
            {
                "username": "frienda",
                "image": "/images/frienda.png",
            },
            {
                "username": "friendb",
                "image": "/images/friendb.png",
            }
        ]
