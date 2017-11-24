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

## Change password

## Forgot password

## Reset password
