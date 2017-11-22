/profile/followers
==================

.. http:get:: /profile/followers

   Obtain current logged in user's followers.

   :<json string token: JWT token of the active user session

   :>json string username: username of follower

   :status 200: when user is logged in; returns list of followers
   :status 401: when the user is not logged in
   :status 404: when user record cannot be found or has been removed

   **Example request**:

   .. sourcecode:: http

       GET /profile/followers HTTP/1.1
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
           "frienda",
           "friendb"
       ]
