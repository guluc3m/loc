# /v1/users

The `users` endpoint implements functionalities related to users and profiles.


## Get user details

Obtain the profile of the specified user

### HTTP Request

`GET /v1/users/profile`

> Example request:

```json
{
    "user": "user32"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| user | string | Username of the user to obtain information about |

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
    "status": "fail",
    "data": {
        "user": "User was not found"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Profile was found, returns shown user attributes |
| 404 | Profile was not found (or removed) |

## Get followers

Get users that follow the specified user.

### HTTP Request

`GET /v1/users/followers`

> Example request:

```json
{
    "user": "user32"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| user | string | Username of the user to obtain information about |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": [
        "friend_a",
        "friend_b",
    ]
}
```

> Example response [404]:

```json
{
    "status": "fail",
    "data": {
        "user": "User was not found"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | User found, returns list of followers |
| 404 | Profile was not found (or removed) |


## Get users followed

Get users followed by the specified user.

### HTTP Request

`GET /v1/users/following`

> Example request:

```json
{
    "user": "user32"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| user | string | Username of the user to obtain information about |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": [
        "friend_a",
        "friend_b",
    ]
}
```

> Example response [404]:

```json
{
    "status": "fail",
    "data": {
        "user": "User was not found"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | User found, returns list of followees |
| 404 | Profile was not found (or removed) |


## Follow/unfollow user

<aside class="notice">
Authenticated endpoint.
</aside>

Follow or unfollow the specified user.

### HTTP Request

`POST /v1/users/follow`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "user": "user31",
    "follow": false
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| user | string | Username of the user to follow/unfollow |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| follow | boolean | Whether to start or stop following the user |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": null
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
    "status": "fail",
    "data": {
        "user": "User was not found"
    }
}
```

> Example response (already following) [409]:

```json
{
    "status": "fail",
    "data": {
        "user": "You are already following that user"
    }
}
```

> Example response (not following) [409]:

```json
{
    "status": "fail",
    "data": {
        "user": "You were not following that user"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | User is followed/unfollowed |
| 401 | Not logged in |
| 404 | Profile was not found (or removed) |
| 409 | The user was already being followed/unfollowed |


## List matches the user is participating in

Obtain a list of matches the user is currently participating in (currently
running or future).

<aside class="success">
The list of matches is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/users/matches`

> Example request:

```json
{
    "user": "user32",
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| user | string | Username of the user to get information about |
| page | integer | Page number to fetch (defaults to `1`) |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "page": 1,
        "pages": 3,
        "list": [
            {
                "title": "Test match",
                "start-date": "2017-01-01 00:00",
                "end-date": "2017-01-05 00:00",
                "slug": "test-match"
            },
            {
                "title": "Super awesome match 9000",
                "start-date": "2017-02-01 00:00",
                "end-date": "2017-02-05 00:00",
                "slug": "super-awesome-match-9000"
            }
        ] 
    }
}
```

> Example response [404]:

```json
{
    "status": "fail",
    "data": {
        "user": "User was not found"
    }
}
```


The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | List of matches (list may be empty if no match is found) |
| 404 | User was not found |


## List matches the user has participated in

Obtain a list of past matches the user has participated in.

<aside class="success">
The list of matches is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/users/past-matches`

> Example request:

```json
{
    "user": "user32",
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| user | string | Username of the user to get information about |
| page | integer | Page number to fetch (defaults to `1`) |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "page": 1,
        "pages": 3,
        "list": [
            {
                "title": "Past match",
                "start-date": "2016-01-01 00:00",
                "end-date": "2016-01-05 00:00",
                "slug": "past-match"
            },
            {
                "title": "Super awesome match 8000",
                "start-date": "2015-02-01 00:00",
                "end-date": "2015-02-05 00:00",
                "slug": "super-awesome-match-8000"
            }
        ] 
    }
}
```

> Example response [404]:

```json
{
    "status": "fail",
    "data": {
        "user": "User was not found"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | List of matches (list may be empty if no match is found) |
| 404 | User was not found |
