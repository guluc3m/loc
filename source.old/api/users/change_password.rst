/profile/change-password
========================

.. http:post:: /profile/change-password

    Changes user password. This change only occurs if the current password is
    correct.

    A session must be started.

    :<json string token: JWT of the active user session
    :<json string current_password: current password
    :<json string new_password: new password

    :status 200: when user is logged in and its user record exists
    :status 401: when the user is not logged in
    :status 404: when user record has been removed

    **Example request**:

    .. sourcecode:: http

        POST /profile/change-password HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "token": "JWT_TOKEN",
            "current_password": "MY_PASSWORD",
            "new_password": "NEW_PASSWORD",
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json


