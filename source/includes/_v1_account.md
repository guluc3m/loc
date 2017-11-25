# /v1/account

The `account` endpoint implements functionalities related to the management of
a user's own account.

## Signup

Create a new user record in the server.

### HTTP Request

`POST /v1/account/signup`

> Example request:

```json
{
    "username": "test_user",
    "email": "my_email@example.com",
    "password": "mypassword"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| username | string | Unique username for the new user |
| email | string | Unique email for the new user |
| password | string | Password for the new user (will be stored encrypted) |

### HTTP Response

> Example response [201]:

```json
{
    "status": "success",
    "data": {
        "username": "test_user"
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "password": "Field was missing in request"
    }
}
```

> Example response [409]:

```json
{
    "status": "error",
    "message": "A user with the specified details already exists"
}
```

> Example response [500]:

```json
{
    "status": "error",
    "message": "Error creating record"
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 201 | User is created correctly, returns newly created user's username |
| 400 | Some parameters are missing (indicated in response) |
| 409 | User with the specified username/email already exists, returns error message |
| 500 | Server error |


## Login

Login using the provided username and password in order to obtain a JSON Web
Token (necessary for most endpoints).

### HTTP Request

`POST /v1/account/login`

> Example request:

```json
{
    "username": "test_user",
    "password": "mypassword"
    "remember_me": false
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| username | string | Username to use for login |
| password | string | Password to use for login |
| remember_me | string | Whether the session should remain active or expire |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "token": "JWT_TOKEN"
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "password": "Field was missing in request"
    }
}
```

> Example response [401]:

```json
{
    "status": "fail",
    "data": {
        "username": "Check field for errors",
        "password": "Check field for errors"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Credentials are valid, returns JWT |
| 400 | Some parameters are missing (indicated in response) |
| 401 | Credentials are not valid |

## Logout

TODO

### HTTP Request

`GET /v1/account/logout`

## Get own profile

<aside class="notice">
Authenticated endpoint.
</aside>

Obtain the profile of the currently logged in user.

### HTTP Request

`GET /v1/account/profile`

> Example request:

```json
{
    "token": "JWT_TOKEN",
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "username": "currentuser",
        "name": "Current user",
        "points": 42
    }
}
```

> Example response [404]:

```json
{
    "status": "error",
    "message": "User was not found"
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Profile was found, returns shown user attributes |
| 404 | Profile was not found (or removed) |


## Update own profile

<aside class="notice">
Authenticated endpoint.
</aside>

Update profile details of the currently logged in user.

### HTTP Request

`PUT /v1/account/profile`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "name": "New User McUser",
    "email": "newmail@example.com"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| name | string | New real name to show (optional) |
| email | string | New email to use for communication with the user (required) |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "name": "New User McUser"
        "email": "newmail@example.com"
    }
}
```

> Example response [404]:

```json
{
    "status": "error",
    "message": "User was not found"
}
```

> Example response [500]:

```json
{
    "status": "error",
    "message": "Error updating record"
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Profile was found, returns new details |
| 404 | Profile was not found (or removed) |
| 500 | Server error |

<aside class="success">
<strong>Note</strong>: if the name is not provided, the current name will be returned in the
response.
</aside>

## Get followers

<aside class="notice">
Authenticated endpoint.
</aside>

Get users that follow the currently logged in user.

### HTTP Request

`GET /v1/account/followers`

> Example request:

```json
{
    "token": "JWT_TOKEN",
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": [
        {
            "username": "friend_a",
            "follow_date": "2017-01-01 00:00"
        },
        {
            "username": "friend_b",
            "follow_date": "2017-01-02 00:00"
        }
    ]
}
```

> Example response [401]:

```json
{
    "status": "error",
    "message": "User not logged in"
}
```

> Example response [404]:

```json
{
    "status": "error",
    "message": "User was not found"
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Logged in, returns list of followers |
| 401 | Not logged in |
| 404 | Profile was not found (or removed) |

## Get users followed

<aside class="notice">
Authenticated endpoint.
</aside>

Get users followed by the currently logged in user.

### HTTP Request

`GET /v1/account/following`

> Example request:

```json
{
    "token": "JWT_TOKEN",
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": [
        {
            "username": "friend_a",
            "follow_date": "2017-01-01 00:00"
        },
        {
            "username": "friend_b",
            "follow_date": "2017-01-02 00:00"
        }
    ]
}
```

> Example response [401]:

```json
{
    "status": "error",
    "message": "User not logged in"
}
```

> Example response [404]:

```json
{
    "status": "error",
    "message": "User was not found"
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Logged in, returns list of followees |
| 401 | Not logged in |
| 404 | Profile was not found (or removed) |

## Change password

<aside class="notice">
Authenticated endpoint.
</aside>

Change current password (**only when the user is logged in**).

### HTTP Request

`POST /v1/account/change-password`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "current_password": "mycurrent_password",
    "new_password": "mynew_password"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| current_password | string | Currently set password |
| new_password | string | New password to set |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {}
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "password": "Field was missing in request"
        "new_password": "Password not complex enough"
    }
}
```

> Example response [401]:

```json
{
    "status": "error",
    "message": "User not logged in"
}
```

> Example response [403]:

```json
{
    "status": "error",
    "message": "Invalid password"
}
```

> Example response [404]:

```json
{
    "status": "error",
    "message": "User was not found"
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Password changed |
| 400 | Some parameters are missing or incomplete (indicated in response) |
| 401 | Not logged in |
| 403 | Current password was incorrect |
| 404 | Profile was not found (or removed) |

## Forgot password

Generate a password reset token.

Note that the endpoint will not specify whether the email address is valid
and belongs to a user account.

### HTTP Request

`POST /v1/account/forgot-password`

> Example request:

```json
{
    "email": "myemail@example.com"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| email | string | Email of the account |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {}
}
```

> Example response [500]:

```json
{
    "status": "error",
    "message": "Server error"
}
```


The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Password reset token generated (if the email exists) |
| 500 | Server error |


## Validate reset password token

Validate the token generated in `/v1/account/forgot-password`.

If this token is valid, then it is possible to make a `POST` request with the
new password.

### HTTP Request

`GET /v1/account/reset-password/<token>`

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | Reset password token |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {}
}
```

> Example response[404]:

```json
{
    "status": "error",
    "message": "Invalid token"
}
```

> Example response [500]:

```json
{
    "status": "error",
    "message": "Server error"
}
```


The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Reset token exists |
| 404 | Reset token was not found |
| 500 | Server error |


## Reset password

Reset password.

While it is possible to skip checking whether the token from
`v1/account/forgot-password` is valid (by performing a `GET` request to this
endpoint), it is not recommended.

<aside class="warning">
<strong>NOTE</strong>: This invalidates all JWT for the specified user.
</aside>

### HTTP Request

`POST /v1/account/reset-password/<token>`

> Example request:

```json
{
    "password": "myresetpassword",
    "confirm_password": "myresetpassword"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | Password reset token |
| password | string | New password |
| confirm_password | string | Password confirmation |


### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {}
}
```

> Example response [400]:

```json
    "status": "fail",
    "data": {
        "password": "Not complex enough",
        "confirm_password": "Passwords do not match"
    }
```

> Example response [500]:

```json
{
    "status": "error",
    "message": "Server error"
}
```


The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Password has been changed |
| 400 | Password was not complex enough or the confirmation did not match |
| 500 | Server error |
