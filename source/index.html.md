---
title: League of Code API Reference


toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - models 
  - v1_account
  - v1_matches
  - v1_parties

search: true
---

# Introduction

Welcome to the League of Code API!

This is the official specification of the online hackathon platform designed
by GUL-UC3M.

# Authentication

Authentication in the League of Code API is performed by means of [JSON Web
Tokens](https://jwt.io/). For each authenticated endpoint, a `token` parameter
containing the current JWT must be included in the request JSON. This token
can be obtained from the [`/v1/account/login`](#login) endpoint.

> Token missing from request [401]:

```json
{
    "status": "fail",
    "data": {
        "token": "Request was missing the 'token' attribute "
    }
}
```

> Token expired [401]:

```json
{
    "status": "error",
    "message": "JWT was expired"
}
```

> Token (de)coding error [500]:

```json
{
    "status": "error",
    "message": "JWT coding error"
}
```

For each authenticated endpoint, the server may reply with a `401` code if the
JWT sent is not valid or a `500` code if there were coding errors while
(de)coding de token.
