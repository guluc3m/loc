/matches/<slug>/enter
===================

.. http:post:: /matches/(string: slug)/enter

    Enter a match.

    Participants need a team to enter a match. These teams are created on demand
    when entering a match and have a unique token that the owner of the group
    can use to invite others to the team.

    The team will **only** participate in the match if `Team.num_members` is
    between the minimum and maximum number of members allowed in a match (
    `Team.is_participating` is changed to true when that condition is satisfied).

    A `TeamMember` record is created for the owner when the team is created, so
    it is necessary to check whether a record for the match already exists.

    :param string slug: unique slug of the match

    :<json string token: JWT token of the active user session

    :>json string invite_token: token used to invite others to the team

    :status 200: when user is logged in and the match is available
    :status 401: when the user is not logged in
    :status 404: when the match does not exist
    :status 409: when a `TeamMember` record already exists for the user-match
        pair

    **Example request**:

    .. sourcecode:: http

        POST /matches/practice-match/enter HTTP/1.1
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

        {
            "invite_token": "abcdefghijk"
        }
