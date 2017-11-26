# /v1/admin

The `admin` endpoint implements functionalities related to the management of the
platform.

<aside class="warning">
All of the following endpoints require an `admin` role in the user account.
</aside>


## Create a match

<aside class="notice">
Authenticated endpoint.
</aside>

Create a new match.

### HTTP Request

`POST /v1/admin/match`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "title": "New match title",
    "short-description": "This is a short description",
    "long-description": "This is a long description",
    "start-date": "2017-01-01 00:00",
    "end-date": "2017-01-05 00:00",
    "min-team": 1,
    "max-team": 5,
    "is-visible": false
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| title | string | Title for the match |
| short-description | string | Short description for the match |
| long-description | string | Long description for the match |
| start-date | date | Starting date(time) of the match |
| end-date | date | Ending date(time) of the match |
| min-team | integer | Minimum number of members required in a party |
| max-team | integer | Maximum number of members allowed in a party |
| is-visible | boolean | Whether the match can be found when querying the matches |

### HTTP Response

> Example response [201]:

```json
{
    "status": "success",
    "data": {
        "slug": "test-match"
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "start-date": "Cannot be empty"
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
    "message": "You are not an administrator"
}
```

> Example response [409]:

```json
{
    "status": "fail",
    "data": {
        "title": "A match with this title already exists"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 201 | Match created, returns match slug |
| 400 | Some parameters are missing or incomplete (indicated in response) |
| 401 | Not logged in |
| 403 | Not an administrator |
| 409 | A match with that slug/title already exists |


## Modify a match

<aside class="notice">
Authenticated endpoint.
</aside>

Modify an existing match.

Note that changing the title also changes the slug.

### HTTP Request

`PUT /v1/admin/match`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "current-match-slug",
    "max-team": 4,
    "is-visible": false
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match to modify |

Every parameter shown in the [Create a match](#create-a-match) endpoint can be
used to modify the specific attribute.

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "slug": "test-match"
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "is-visible": "Invalid type"
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
    "message": "You are not an administrator"
}
```

> Example response [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match not found"
    }
}
```


> Example response [409]:

```json
{
    "status": "fail",
    "data": {
        "title": "A match with this title already exists"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Match modified, returns match slug |
| 400 | Some parameters are missing or incomplete (indicated in response) |
| 401 | Not logged in |
| 403 | Not an administrator |
| 404 | Match does not exist |
| 409 | A match with that slug/title already exists |


## Delete/Undelete a match

<aside class="notice">
Authenticated endpoint.
</aside>

Delete/Undelete a match.

This is a soft delete and can be undone.


### HTTP Request

`PUT /v1/admin/match-delete`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "current-match-slug",
    "delete": true
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match to delete |
| delete | boolean | Whether to delete (true) or undelete (false) a match |

### HTTP Response

> Example response (delete) [200]:

```json
{
    "status": "success",
    "data": {
        "delete-date": "2017-10-10 15:50"
    }
}
```

> Example response (undelete) [200]:

```json
{
    "status": "success",
    "data": {
        "match": "restored-match"
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
    "message": "You are not an administrator"
}
```

> Example response [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match not found"
    }
}
```

> Example response [409]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match was already deleted"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Match deleted/undeleted, returns deletion date (when deleting) or match slug (when undeleting) |
| 401 | Not logged in |
| 403 | Not an administrator |
| 404 | Match does not exist |
| 409 | The match was already deleted/undeleted |


## List deleted matches

<aside class="notice">
Authenticated endpoint.
</aside>

Obtain a list of deleted matches.

<aside class="success">
The list of matches is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/admin/deleted-matches`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
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
                "delete-date": "2017-01-05 00:00",
                "slug": "test-match"
            },
            {
                "title": "Super awesome match 9000",
                "delete-date": "2017-02-01 00:00",
                "slug": "super-awesome-match-9000"
            }
        ] 
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
    "message": "You are not an administrator"
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | List of matches |
| 401 | Not logged in |
| 403 | Not an administrator |

## List users

<aside class="notice">
Authenticated endpoint.
</aside>

Obtain a complete list of active users.

<aside class="success">
The list of users is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/admin/users`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| page | integer | Page number to fetch (defaults to `1`) |


### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "page": 1,
        "pages": 1,
        "list": [
            "user1",
            "user2",
            "user3",
            "user4"
        ]
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
    "message": "You are not an administrator"
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | List of users | 
| 401 | Not logged in |
| 403 | Not an administrator |

## List deleted/banned users

<aside class="notice">
Authenticated endpoint.
</aside>

Obtain a list of banned/deleted users.

<aside class="success">
The list of users is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/admin/deleted-users`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| page | integer | Page number to fetch (defaults to `1`) |


### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "page": 1,
        "pages": 1,
        "list": [
            "user1",
            "user2",
            "user3",
            "user4"
        ]
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
    "message": "You are not an administrator"
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | List of users | 
| 401 | Not logged in |
| 403 | Not an administrator |

## Ban/unban user

<aside class="notice">
Authenticated endpoint.
</aside>

Ban/Unban a user.

### HTTP Request

`PUT /v1/admin/user-delete`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "user": "username-to-ban",
    "delete": true
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| user | string | Username of the user to ban |
| delete | boolean | Whether to delete (true) or undelete (false) a match |

### HTTP Response

> Example response (delete) [200]:

```json
{
    "status": "success",
    "data": {
        "delete-date": "2017-10-10 15:50"
    }
}
```

> Example response (undelete) [200]:

```json
{
    "status": "success",
    "data": {
        "user": "username-to-delete"
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
    "message": "You are not an administrator"
}
```

> Example response [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match not found"
    }
}
```

> Example response [409]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match was already deleted"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | User deleted/undeleted, returns deletion date (when deleting) or username (when undeleting) |
| 401 | Not logged in |
| 403 | Not an administrator |
| 404 | User does not exist |
| 409 | The user was already deleted/undeleted |
