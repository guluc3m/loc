/reset-password
===============

.. http:post:: /reset-password

    Send an email with a link that the user can use to reset their password.

    No session must be started.

    Sets an unique ``password_reset_token`` in the ``User`` model with expiration
    date.

    :<json string email: the email to which the link should be sent

    :status 200: when user record with that email exists
    :status 403: when the user is logged in
    :status 404: when user record has been removed or does not exist

    **Example request**:

    .. sourcecode:: http

        POST /forgot-password HTTP/1.1
        Host: example.com
        Accept: application/json
        Content-Type: application/json

        {
            "email": "user@example.com"
        }

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

