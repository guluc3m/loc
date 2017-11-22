/matches/<slug>
===============

.. http:get:: /matches/(string: slug)

    Obtain complete details for a match.

    :param string slug: unique slug of the match

    :>json int id: unique id of the match
    :>json string title: title of the match
    :>json string short_description: short description of the match
    :>json string long_description: long description of the match.
        This is Markdown enabled, but will be empty unless the match has
        already started
    :>json date start_date: start date of the match
    :>json date end_date: end date of the match
    :>json int min_team: minimum number of participants for each team
    :>json int max_team: maximum number of participants for each team
    :>json string slug: unique slug of the match

    :status 200: when record was found; returns match details
    :status 404: when record was not found or it was deleted

    **Example request**:

    .. sourcecode:: http

        GET /matches/practice-match HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "id": 42,
            "title": "Practice match",
            "short_description": "This is a practice match",
            "long_description": "This is a practice match. LONG DESC",
            "start_date": "2016-04-23T18:25:43.511Z",
            "end_date": "2016-05-15T18:25:43.511Z",
            "min_team": 2,
            "max_team": 4,
            "slug": "practice-match"
        }


.. http:put:: /matches/(string: slug)

    Modify the details of a match.

    This endpoint requires the *admin* role, therefore the server will check
    the supplied JWT and the stored role.

    :param string slug: unique slug of the match

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
    :>json string short_description: short description of the match
    :>json string long_description: long description of the match (Markdown
        enabled)
    :>json date start_date: start date of the match
    :>json date end_date: end date of the match
    :>json int min_team: minimum number of participants for each team
    :>json int max_team: maximum number of participants for each team
    :>json string slug: unique slug for the match
    :>json boolean is_visible: whether the match is visible the public

    :status 200: when record was found; returns updated match details
    :status 401: when the user does not have the required admin role
    :status 404: when record was not found or it was deleted

    **Example request**:

    .. sourcecode:: http

        PUT /matches/practice-match HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "token": "JWT_TOKEN",

            "title": "Updated Practice match",
            "short_description": "This is a practice match, updated",
            "long_description": "This is the long description, updated"
            "start_date": "2016-04-23T18:25:43.511Z",
            "end_date": "2016-05-15T18:25:43.511Z",
            "min_team": 1,
            "max_team": 1,
            "is_visible": false
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "id": 42,
            "title": "Updated Practice match",
            "short_description": "This is a practice match, Updated",
            "long_description": "This is the long description, updated"
            "start_date": "2016-04-23T18:25:43.511Z",
            "end_date": "2016-05-15T18:25:43.511Z",
            "min_team": 1,
            "max_team": 1,
            "is_visible": false
        }

