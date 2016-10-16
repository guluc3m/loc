/matches
========

.. http:get:: /matches

    Obtain a list of Matches that have taken place or are currently taking place.

    Does not return invisible or deleted matches.

    :>json int id: unique id of the match
    :>json string title: title of the match
    :>json date start_date: start date of the match
    :>json date end_date: end date of the match
    :>json string slug: slug of the match

    :status 200: a list is always returned, although it may be empty

    **Example request**:

    .. sourcecode:: http

        GET /matches HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        [
            {
                "id": 42,
                "title": "Practice match",
                "start_date": "2016-04-23T18:25:43.511Z",
                "end_date": "2016-05-15T18:25:43.511Z",
                "slug": "practice-match",
            }
        ]


.. http:post:: /matches

    Create a new match.

    This endpoint requires the *admin* role, therefore the server will check
    the supplied JWT and the stored role.

    :<json string token: JWT of the active user session
    :<json string title: title of the match
    :<json string short_description: short description of the match
    :<json string long_description: long description of the match (Markdown
        enabled)
    :<json date start_date: start date of the match
    :<json date end_date: end date of the match
    :<json int min_team: minimum number of participants for each team
    :<json int max_team: maximum number of participants for each team
    :<json string slug: unique slug for the match; can be created automatically
        if it is not provided
    :<json boolean is_visible: whether the match is visible the public

    :>json int id: unique id of the match
    :>json string title: title of the match
    :>json date start_date: start date of the match
    :>json date end_date: end date of the match
    :>json string slug: unique slug of the match

    :status 201: when the record is created
    :status 401: when the user does not have the required role
    :status 409: when a match with the provided details already exists

    **Example request**:

    .. sourcecode:: http

        POST /matches HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "token": "JWT_TOKEN",

            "title": "New Practice match",
            "short_description": "This is a practice match",
            "long_description": "This is the long description"
            "start_date": "2016-04-23T18:25:43.511Z",
            "end_date": "2016-05-15T18:25:43.511Z",
            "min_team": 1,
            "max_team": 3,
            "is_visible": true
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        [
            {
                "id": 42,
                "title": "Practice match",
                "start_date": "2016-04-23T18:25:43.511Z",
                "end_date": "2016-05-15T18:25:43.511Z",
                "slug": "practice-match"
            }
        ]
