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
    "min-members": 1,
    "max-members": 5,
    "is-visible": false,
    "slug": "custom-slug"
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
| min-members | integer | Minimum number of members required in a party |
| max-members | integer | Maximum number of members allowed in a party |
| is-visible | boolean | Whether the match can be found when querying the matches |
| slug | str | Optional slug to override the one generated from the title |

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
    "leaderboard": true,
    "is-visible": false
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match to modify |
| leaderboard | boolean | (Optional) Whether to show the leaderboard |

Every parameter shown in the [Create a match](#create-a-match) endpoint can be
used to modify the specific attribute.

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "start-date": "2017-10-10 10:00:00",
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
| 200 | Match modified, returns new match details |
| 400 | Some parameters are missing or incomplete (indicated in response) |
| 401 | Not logged in |
| 403 | Not an administrator |
| 404 | Match does not exist |
| 409 | A match with that slug/title already exists |


## Get match leaderboard

<aside class="notice">
Authenticated endpoint.
</aside>

Get the current leaderboard of the specified match.

This endpoint does not check if the leaderboard is posted or not.

### HTTP Request

`GET /v1/admin/match-leaderboard`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "test-match",
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match to get the leaderboard for |
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
                "leader": "user32",
                "members": ["user32", "userc"],
                "position": 1
            },
            {
                "leader": "userc",
                "members": ["userc"],
                "position": 2
            }
        ] 
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Ordered party list including the position of the party |



## Set match leaderboard

<aside class="notice">
Authenticated endpoint.
</aside>

Update the leaderboard of the match by setting positions for each participating
party.

Note that this only sets the position for the specified parties, and will not
modify already set positions in other parties.

### HTTP Request

`PUT /v1/admin/match-leaderboard`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "current-match-slug",
    "positions": [
        {
            "party": "party-leader-1",
            "position": 2,
        },
        {
            "party": "party-leader-3",
            "position": 1,
        }
    ]
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match to delete |
| positions | list[json] | List of objects specifying the party to update and its new position |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": [
        "party-leader1", "party-leader-2"
    ]
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

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Match leaderboard modified, returns list of modified parties |
| 401 | Not logged in |
| 403 | Not an administrator |
| 404 | Match does not exist |


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
            {
                "username": "user1",
                "delete-date": "2017-01-01 10:00:00"
            },
            {
                "username": "user2",
                "delete-date": "2017-01-01 10:00:00"
            },
            {
                "username": "user3",
                "delete-date": "2017-01-01 10:00:00"
            },
            {
                "username": "user4",
                "delete-date": "2017-01-01 10:00:00"
            },
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
