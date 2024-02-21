# Connect API - Basics

## Authorization

The connect api use a token authentication (bearer authentication) with a json web token (JWT). For JWT generation you get a issuer and private key from our side. With this you can generate the necessary token for the authentication on your side.

```
GET /connect/drop-slot/balance/v1
Content-type: application/json
Authorization: Bearer {{ TOKEN }}
```

Following properties are mandatory in the token payload:

| Field | Type | Description |
| --- | --- | --- |
| `iss` | String | Issuer of the token. Provided from us |
| `sub` | String | Subject/target of the request (e.g. user id or session id) |
| `aud` | String[] | One or more audiences, that the token want use |
| `exp` | Integer | Expire time of the token as unix timestamp. A token could not be longer valid as 5 minutes |
| `iat` | Integer | Create time of the token as unix timestamp |

Example of token payload:

```json
{
  "iss":"issuer.example",
  "aud":[
    "drop-slot/balance"
  ],
  "sub":"test-123-456-789",
  "exp":1707382860,
  "iat":1707382800
}
```

## Response Structure

The response object usually has 3 properties:

| Field | Type | Description |
| --- | --- | --- |
| `r` | Boolean | Response status. `true` if the request was successful. `false` if a error has occurred |
| `data` | Object \| Object[] \| Null | The response payload on success. Otherwise `null` |
| `error` | Object \| Null | Error details when the request was failed. Otherwise `null` |

The error object has 2 properties:

| Field | Type | Description |
| --- | --- | --- |
| `code` | Integer | Error code |
| `message` | String | Message about the error |

Example for a success response:

```json
{
    "r":true,
    "data":{
        "user":{
            "id":"0d5a9c6e-7439-4680-9c2a-145b5dbe28ac",
            "username":"DefensiveBeige#3584",
            "country":"DE",
            "locale":"de-DE"
        }
    }
}
```

Example for a error/falied response:

```json
{
    "r": false,
    "error": {
        "code": 401,
        "message": "Audience is missing in token."
    },
    "data": null
}
```
