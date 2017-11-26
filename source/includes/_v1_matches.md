# /v1/matches

The `matches` endpoint implements functionalities related to code matches.


## List current matches

Obtain a list of matches that are currently taking place or will take place
in the future.

This does not include invisible or deleted matches.

<aside class="success">
The list of matches is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/matches/list`

> Example request:

```json
{
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
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
                "start_date": "2017-01-01 00:00",
                "end_date": "2017-01-05 00:00",
                "slug": "test-match"
            },
            {
                "title": "Super awesome match 9000",
                "start_date": "2017-02-01 00:00",
                "end_date": "2017-02-05 00:00",
                "slug": "super-awesome-match-9000"
            }
        ] 
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | List of matches (list may be empty if no match is found) |


## List past matches

Obtain a list of matches that have already taken place.

This does not include invisible or deleted matches.

<aside class="success">
The list of matches is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/matches/list-past`

> Example request:

```json
{
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
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
                "title": "Old McOld Match",
                "start_date": "2015-01-01 00:00",
                "end_date": "2015-01-05 00:00",
                "slug": "old-mcold-match"
            },
            {
                "title": "Super awesome match 8000",
                "start_date": "2016-02-01 00:00",
                "end_date": "2016-02-05 00:00",
                "slug": "super-awesome-match-8000"
            }
        ] 
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | List of matches (list may be empty if no match is found) |


## Get details of a given match

Obtain details for the specified match (current or old).

Depending on the starting date of the match, the server may not include the
long description of the match if it has not yet started.

### HTTP Request

`GET /v1/matches/info`

```json
{
    "match": "test-match"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| match | string | Unique slug of the match to get info from |

### HTTP Response

> Example response with a match that has started (or happened in the past) [200]:

```json
{
    "status": "success",
    "data": {
        "title": "Present match",
        "short_description": "Prepare for the future",
        "long_description": "This is a long description of the match"
        "start_date": "2017-01-01 00:00",
        "end_date": "2017-01-05 00:00",
        "min_team": 1,
        "max_team": 2,
        "slug": "present-match"
    }
}
```

> Example response with a match that has not started [200]:

```json
{
    "status": "success",
    "data": {
        "title": "Future match",
        "short_description": "Prepare for the future",
        "start_date": "2019-01-01 00:00",
        "end_date": "2019-01-05 00:00",
        "min_team": 1,
        "max_team": 4,
        "slug": "future-match"
    }
}
```

> Example response  [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "Could not find match"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Match exists, returns match details |
| 404 | Match does not exist, is not visible or was deleted |


## Join a match

<aside class="notice">
Authenticated endpoint.
</aside>

By joining a match, a [`MatchParticipant`](#matchparticipant) record is created
for the user in the specified match. By default, the user will be in their own
party, therefore the `party_owner_id` will be the user ID.

In addition, a [`PartyToken`](#partytoken) is also created. This token can be
used to invite other users to your own party. It is deleted once the user joins
a party owned by another user.

### HTTP Request

`POST /v1/match/join`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "test-match"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match to get info from |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "party-token": "123456789abcdefgh"
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match slug missing"
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
        "match": "You are already participating in the match"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | User joined the match, returns party token |
| 400 | Some parameters are missing or incomplete (indicated in response) |
| 401 | Not logged in |
| 404 | Match was not found (or is invisible or removed) |
| 409 | The user already joined the match in the past |


## Leave a match

<aside class="notice">
Authenticated endpoint.
</aside>

Leave a match before it begins. This implies that the user will automatically
leave any party they are in and their [`MatchParticipant`](#matchparticipant)
record will be deleted.

### HTTP Request

`POST /v1/match/leave`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "test-match"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match to get info from |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": null 
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match slug missing"
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
        "match": "You were not participating in the match"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | User left the match (and any party they were in) |
| 400 | Some parameters are missing or incomplete (indicated in response) |
| 401 | Not logged in |
| 404 | Match was not found (or is invisible or removed) |
| 409 | The user already left the match in the past |


## List participating parties

Obtain a list of parties participating in the specified match.

This list is only accessible **once the match has started (or finished)** and
all participating parties have been formed.

<aside class="success">
The list of parties is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/matches/participants`

> Example request:

```json
{
    "match": "test-match"
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| match | string | Unique slug of the match |
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
                "leader": "user1",
                "members": ["user1", "user2", "user3", "user4"]
            },
            {
                "leader": "user8",
                "members": ["user8", "user9", "user11", "user24"]
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
        "match": "Match not found"
    }
}
```


The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | List of participating parties (list may contain only one user if it is an individual party) |
| 404 | Match was not found (or is invisible or removed) |


## List parties looking for more members

Obtain a list of parties looking for more members through the *Looking For Group*
functionality.

### HTTP Request

`GET /v1/matches/lfg`

> Example request:

```json
{
    "match": "test-match"
    "page": 1
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| match | string | Unique slug of the match |
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
                "leader": "user16",
                "members": ["user16"]
            },
            {
                "leader": "user48",
                "members": ["user45", "user91"]
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
        "match": "Match not found"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | List of parties looking for more members |
| 404 | Match was not found (or is invisible or removed) |


## Get submission details

<aside class="notice">
Authenticated endpoint.
</aside>

Get a party's submission for the given match. This action can only be performed
by **any member of the party** during the match and by **anyone** once the
match is over (for historic purposes).

### HTTP Request

`GET /v1/matches/submission`

> Example request during a match:

```json
{
    "token": "JWT_TOKEN",
    "match": "test-match"
}
```

> Example request during a match:

```json
{
    "match": "test-match"
    "party": 42
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match |
| party | integer | (Optional) ID of the leader of the party |

**Note**: the JWT can be omitted if accessing submissions after the match,
and the party parameter can be skipped if requesting the user's own party
submission (both during and after the match), but it is necessary to access
submissions from other parties after the match.


### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "title": "Super submission 9000",
        "description": "This is an awesome submission that simply works",
        "url": "https://github.com/guluc3m"
    }
}
```

> Example response (missing match) [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match not found"
    }
}
```

> Example response (missing party) [404]:

```json
{
    "status": "fail",
    "data": {
        "party": "Party not found for the specified match"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Party submission details |
| 404 | Match or party were not found (or invisible or removed) |


## Edit submission

<aside class="notice">
Authenticated endpoint.
</aside>

Edit a party's submission for the given match. This action can only be performed
**by the party leader**, which is the owner of the party in the case of teams and
the user in the case of individual matches.

The submission can only be edited until the closing date of the match.

### HTTP Request

`PUT /v1/matches/submission`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "test-match",
    "title": "New title",
    "description": "New description",
    "url": "New URL"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match |
| title | string | New title for the submission |
| description | string | New description for the submission |
| url | string | New URL for the submission |

### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "title": "New title",
        "description": "New description",
        "url": "New URL"
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
    "status": "fail",
    "data": {
        "match": "You are not the party leader for this match"
    }
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
| 200 | New submission details |
| 401 | User not logged in |
| 403 | The user is not the party leader and cannot change the submission |
| 404 | Match was not found (or is invisible or removed) |
