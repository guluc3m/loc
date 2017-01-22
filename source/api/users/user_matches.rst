/users/<user>/matches
=====================

.. http:get:: /users/(string: username)/matches

    Show matches in which the user has participated.

    :param string username: the username for which to obtain information

    :>json int team_id: unique id of the team of which the user is member
    :>json int match_id: unique id of the match
    :>json string title: title of the match
    :>json string slug: slug of the match

    :status 200: when user record was found. Returns user information
    :status 404: when user record was not found or was deleted

    **Example request**:

    .. sourcecode:: http

        GET /users/testuser/matches HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        [
            {
                "team_id": 1,
                "match_id": 42,
                "title": "Practice match",
                "slug": "practice-match"
            }
        ]

