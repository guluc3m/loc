/admin/matches/<id>
===================

.. http:get:: /admin/matches/(int: id)

    Obtain complete details for a match.

    :param int id: unique id of the match

    :<json string token: JWT token of the active user session

    :>json int id: unique id of the match
    :>json string title: title of the match
    :>json string description: long description of the match (Markdown enabled)
    :>json date start_date: start date of the match
    :>json date end_date: end date of the match
    :>json int min_team: minimum number of participants for each team
    :>json int max_team: maximum number of participants for each team
    :>json boolean visible: whether the match is visible to the public or not

    :status 200: when record was found; returns match details
    :status 401: when the user does not have the required admin role
    :status 404: when record was not found or it was deleted

    **Example request**:

    .. sourcecode:: http

        GET /admin/matches/42 HTTP/1.1
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

        {
            "id": 42,
            "title": "Practice match",
            "description": "This is a practice match",
            "start_date": "2016-04-23T18:25:43.511Z",
            "end_date": "2016-05-15T18:25:43.511Z",
            "min_team": 2,
            "max_team": 4,
            "visible": false
        }

