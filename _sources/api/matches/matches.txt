/matches
========

.. http:get:: /matches

    Obtain a list of Matches that have taken place or are currently taking place.

    :>json int id: unique id of the match
    :>json string title: title of the match
    :>json date start_date: start date of the match
    :>json date end_date: end date of the match

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
            }
        ]

