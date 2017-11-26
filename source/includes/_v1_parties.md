# /v1/parties

The `parties` endpoint implements functionalities related to match parties.


## Join a party

<aside class="notice">
Authenticated endpoint.
</aside>

Join a party by using the invite token provided by the party leader, or found
through LFG if the party is public.

By joining a party, the `PartyToken` record created for the user (technically
in their own party) is deleted, and the `party_owner_id` of the
`MatchParticipant` record is set to the ID of the leader of the new party.

### HTTP Request

`POST /v1/parties/join`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "party": "123456789abcdefgh"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| party | string | Unique party token |


### HTTP Response

> Example response [201]:

```json
{
    "status": "success",
    "data": {
        "members": ["usera", "userb", "userc"]
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "party": "The party cannot accept any more members"
    }
}
```

> Example response [404]:

```json
{
    "status": "fail",
    "data": {
        "party": "Party not found"
    }
}
```

> Example response [409]:

```json
{
    "status": "fail",
    "data": {
        "party": "The party is full"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 201 | Joined the party, returns names of party members |
| 400 | Trying to join a party from a past match (or a running match) |
| 404 | Party was not found (or invisible or removed) |
| 409 | Party is full |

Note that a party is full when it has a number of members equal to the
`max_members` attribute of the match.


## Leave a party

<aside class="notice">
Authenticated endpoint.
</aside>

Leave a party. This can only be performed by non-leader members of the party
and before the match starts.

When the user leaves a party, a new `PartyToken` record is created to indicate
that they are in their own party, and the `party_owner_id` in their
`MatchParticipant` record is set to their own ID.


### HTTP Request

`POST /v1/parties/leave`

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
| match | string | Unique slug of the match the party is participating in |


### HTTP Response

> Example response [201]:

```json
{
    "status": "success",
    "data": {
        "party-token": "asdfghjkl12345"
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "match": "The match has already started"
    }
}
```

> Example response [403]:

```json
{
    "status": "fail",
    "data": {
        "match": "You are the leader of the party and cannot leave it"
    }
}
```

> Example response (match not found) [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match was not found"
    }
}
```

> Example response (not participating) [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "You are not participating in this match"
    }
}
```


The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 201 | Left the party, returns new party token |
| 400 | Trying to leave a party from a past match (or a running match) |
| 403 | Trying to leave a party while being the leader |
| 404 | Match was not found or user not participating  |


## Kick member from party

<aside class="notice">
Authenticated endpoint.
</aside>

<aside class="warning">
Only the party leader can perform this action.
</aside>

Kick another member from a party.

By kicking them, a `PartyToken` record is created for the kicked member (as
they are now considered to be in their own party), and the `party_owner_id`
attribute in their `MatchParticipant` record is set to their own ID.

### HTTP Request

`POST /v1/parties/kick`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "test-match",
    "user": "member1"
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match the party is participating in |
| user | string | Username of the user to kick |


### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "members": ["member2", "userb"]
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "match": "The match has already started"
    }
}
```

> Example response (kicking oneself) [403]:

```json
{
    "status": "fail",
    "data": {
        "match": "You are the leader of the party and cannot kick yourself"
    }
}
```

> Example response (not leader) [403]:

```json
{
    "status": "fail",
    "data": {
        "match": "You are not the leader of the party"
    }
}
```

> Example response (match not found) [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match was not found"
    }
}
```

> Example response (not participating) [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "You are not participating in this match"
    }
}
```

> Example response (not in party) [404]:

```json
{
    "status": "fail",
    "data": {
        "user": "User could not be found in your party"
    }
}
```


The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Kicked member from party, returns list of current members |
| 400 | Trying to kick from a party from a past match (or a running match) |
| 403 | Trying to kick while not being the leader or trying to kick oneself |
| 404 | Match was not found or user not participating, or user not in party |



## Disband party

<aside class="notice">
Authenticated endpoint.
</aside>

<aside class="warning">
Only the party leader can perform this action.
</aside>

Kicks all members of the party and generates a new token for the party leader,
effectively removing the party and the invite token. The visibility of the
party is set to private so it cannot be found through LFG.


### HTTP Request

`POST /v1/parties/disband`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "test-match",
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | Unique slug of the match the party is participating in |


### HTTP Response

> Example response [201]:

```json
{
    "status": "success",
    "data": {
        "party-token": "zxcvbnmqwerty"
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "match": "The match has already started"
    }
}
```

> Example response [403]:

```json
{
    "status": "fail",
    "data": {
        "match": "You are not the leader of the party"
    }
}
```

> Example response (match not found) [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match was not found"
    }
}
```

> Example response (not participating) [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "You are not participating in this match"
    }
}
```


The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Kicked member from party, returns list of current members |
| 400 | Trying to disband a party from a past match (or a running match) |
| 403 | Trying to disband while not being the leader |
| 404 | Match was not found or user not participating |


## Set visibility of party in LFG

<aside class="notice">
Authenticated endpoint.
</aside>

<aside class="warning">
Only the party leader can perform this action.
</aside>

Change the visibility of the party in the LFG section. If set to `false` the
party will not appear in the LFG list.

### HTTP Request

`POST /v1/parties/lfg`

> Example request:

```json
{
    "token": "JWT_TOKEN",
    "match": "test-match",
    "lfg": true
}
```

### Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| token | string | JSON Web Token (obtained from `/v1/account/login`) |
| match | string | The match the party is participating in |
| lfg | boolean | Whether the party should appear in the LFG list |


### HTTP Response

> Example response [200]:

```json
{
    "status": "success",
    "data": {
        "lfg": true
    }
}
```

> Example response [400]:

```json
{
    "status": "fail",
    "data": {
        "match": "The match has already started"
    }
}
```

> Example response [403]:

```json
{
    "status": "fail",
    "data": {
        "match": "You are not the leader of the party"
    }
}
```

> Example response (match not found) [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "Match was not found"
    }
}
```

> Example response (not participating) [404]:

```json
{
    "status": "fail",
    "data": {
        "match": "You are not participating in this match"
    }
}
```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Returns new LFG setting |
| 400 | Trying to change the LFG setting for a party from a past match (or a running match) |
| 403 | Trying to change LFG setting while not being the leader |
| 404 | Match was not found or user not participating |



## List parties the user is currently in

<aside class="notice">
Authenticated endpoint.
</aside>

Obtain a list of parties the user is currently a member of (for running or
future matches). Does not include invisible or deleted matches.

<aside class="success">
The list of matches is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/parties/list`

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
                "match": {
                    "title": "Test match",
                    "start-date": "2017-01-01 00:00",
                    "end-date": "2017-01-05 00:00",
                    "slug": "test-match"
                },
                "party": {
                    "leader": "user32",
                    "members": ["user32", "userc"],
                    "party-token": "asdfghjklqwerty"
                }
            },
            {
                "match": {
                    "title": "Super match",
                    "start-date": "2018-01-01 00:00",
                    "end-date": "2018-01-05 00:00",
                    "slug": "super-match"
                },
                "party": {
                    "leader": "userc",
                    "members": ["userc"],
                    "party-token": "1234567890asdfghjkl"
                }
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
| 200 | List of parties |
| 401 | Not logged in |
| 404 | Profile was not found (or removed) |


## List parties the user has been in

<aside class="notice">
Authenticated endpoint.
</aside>

Obtain a list of parties the user has been a member of. Does not include
invisible or deleted matches.

<aside class="success">
The list of matches is paginated and the response includes the current page
as well as the total number of pages.
</aside>

### HTTP Request

`GET /v1/parties/list-past`

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
        "pages": 2,
        "list": [
            {
                "match": {
                    "title": "Past match",
                    "start-date": "2016-01-01 00:00",
                    "end-date": "2016-01-05 00:00",
                    "slug": "past-match"
                },
                "party": {
                    "leader": "user32",
                    "members": ["user32", "userc"],
                    "party-token": "asdfghjklqwerty"
                }
            },
            {
                "match": {
                    "title": "Super match 7000",
                    "start-date": "2015-01-01 00:00",
                    "end-date": "2015-01-05 00:00",
                    "slug": "super-match-7000"
                },
                "party": {
                    "leader": "userc",
                    "members": ["userc"],
                    "party-token": "1234567890asdfghjkl"
                }
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
| 200 | List of parties |
| 401 | Not logged in |
| 404 | Profile was not found (or removed) |
