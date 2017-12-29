# Common endpoints

The following endpoints are common and independent of the API version used.


## Obtain server information

Get certain details from the server. This includes the following:

- Short server description
- Server version
- Implemented API versions

### HTTP Request

`GET /info`

### HTTP Response

> Example response:

```json
{
    "api": [
        "v1"
    ],
    "description": "Reference LoC server implementation",
    "version": "0.1.0"
}

```

The following HTTP codes can be returned by this endpoint:

| HTTP Code | Description |
| --- | --- |
| 200 | Returns server information |
