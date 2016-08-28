/matches/<id>/enter
===================

TODO: add error status code

.. http:post:: /matches/(int: id)/enter

    Enter a match.

    Participants need a team to enter a match. These teams are created on demand
    when entering a match and each user, except for the one that created the team,
    receives an invitation to participate as part of the team.

    Only when all members have accepted is the team participating in the match.

    :param int id: unique id of the match

    :<json string token: JWT token of the active user session
    :<json list[string] members: list of usernames (without the user creating the
        team) to invite to the team

    :status 200: when user is logged in and all the users are available
    :status 401: when the user is not logged in

    **Example request**:

    .. sourcecode:: http

        POST /matches/42/enter HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "token": "JWT_TOKEN",
            "members": [
                "user1",
                "user2",
                "user3"
            ]
        }
