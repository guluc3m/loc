/profile/following
==================

.. http:get:: /profile/following

   Obtain current logged in user is following.

   :<json string token: JWT token of the active user session

   :>json string username: username followed

   :status 200: when user is logged in; returns list of followed users
   :status 401: when the user is not logged in
   :status 404: when user record cannot be found or has been removed

   **Example request**:

   .. sourcecode:: http

       GET /profile/following HTTP/1.1
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
