/admin/matches
==============

.. http:get:: /matches

    Obtain a list of Matches that have taken place or are currently taking place.

    Does not return deleted matches.

    **Requires ``admin`` role**.

    :<json string token: JWT token of the active user session

    :>json int id: unique id of the match
    :>json string title: title of the match
    :>json date start_date: start date of the match
    :>json date end_date: end date of the match
    :>json boolean visible: whether the match is visible to the public or not

    :status 200: a list is always returned, although it may be empty
    :status 401: when the user does not have the required admin role

    **Example request**:

    .. sourcecode:: http

        GET /admin/matches HTTP/1.1
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
                "id": 42,
                "title": "Practice match",
                "start_date": "2016-04-23T18:25:43.511Z",
                "end_date": "2016-05-15T18:25:43.511Z",
                "visible": false
            }
        ]


