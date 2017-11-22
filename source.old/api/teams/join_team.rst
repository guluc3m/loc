/teams/join/<token>
===================

.. http:get:: /teams/join/(string: token)

   Check if an invitation token is valid.

   This should check if the team is already complete by comparing the number
   of members to the maximum allowed in the match.

   :param string token: unique invitation token

   :<json string token: JWT token of the active user session

   :>json boolean is_full: whether the team is already full or not

   :status 200: when user is logged in and the token is valid, includes a
       message indicating if it is possible to join the team
   :status 401: when the user is not logged in
   :status 404: when the token does not exist
   :status 409: when a `TeamMember` record already exists for the user-match
       pair

   **Example request**:

   .. sourcecode:: http

       GET /teams/join/abcdefghijk/ HTTP/1.1
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
           "status" : "success",
           "data" : {
               "is_full": false
           }
       }

.. http:post:: /teams/join/(string: token)

   Join a team.

   This should check if the team is already complete by comparing the number
   of members to the maximum allowed in the match.

   :param string token: unique invitation token

   :<json string token: JWT token of the active user session

   :status 200: when user is logged in and the token is valid, includes a
       message indicating if it is possible to join the team
   :status 401: when the user is not logged in
   :status 404: when the token does not exist
   :status 409: when a `TeamMember` record already exists for the user-match
       pair

   **Example request**:

   .. sourcecode:: http

       GET /teams/join/abcdefghijk/ HTTP/1.1
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
