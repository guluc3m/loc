/reset-password/<token>
=======================

.. http:get:: /reset-password/(string: token)

    Validate the given password reset token.

    :param string token: unique token used to reset password

    :status 200: when user record with that token exists and is valid
    :status 403: when the user is logged in
    :status 404: when user record has been removed, does not exist or token is
        not valid

    **Example request**:

    .. sourcecode:: http

        GET /reset-password/MY_TOKEN HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json


    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json


.. http:post:: /reset-password/(string: token)

    Change the password for a specific record.

    This also removes the password reset token from the record.

    :param string token: unique token used to reset password

    :<json string password: new password for the user

    :status 200: when the password is changed successfully
    :status 403: when the user is logged in
    :status 404: when user record has been removed, does not exist or token is
        not valid

    **Example request**:

    .. sourcecode:: http

        POST /reset-password/MY_TOKEN HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "password": "my new password"
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

