# Connect API - Drop Slot

## Types & Status

### Authorization Audiences

| Audience | Description |
| --- |  --- |
| `drop-slot/balance` | Allowed to request the current balance of the user |
| `drop-slot/bet` | Allowed to make a bet for the user |
| `drop-slot/close` | Allowed to close a round for the user |
| `drop-slot/item-drop` | Allowed to trigger a item drop for the user |
| `drop-slot/revert` | Allowed to revert the round bet(s) for the user |

## Error Codes

| Code | Description |
| --- | --- |
| `400` | Request invalid or not well formed |
| `401` | Authentication error |
| `403` | User is blocked or round not belong to user |
| `404` | Slot session, round or user not found |
| `410` | Slot Session or round was closed |
| `413` | User has no or not enough coins in his wallet |
| `507` | Data saving on the server has failed |

### Round Status

| Status | Description |
| --- |  --- |
| `pending` | Round is open for more bets or could be closed or reverted |
| `close` | Round is closed and finished |
| `reverted` | Round is reverted and finished |

## API Objects

### User Object

| Field | Type | Description |
| --- | --- | --- |
| `userId` | String | ID of the user |
| `username` | String | User nickname |
| `country` | String | Code of the user country |
| `locale` | String | Locale of the user |

```json
{
    "userId":"0d5a9c6e-7439-4680-9c2a-145b5dbe28ac",
    "username":"DefensiveBeige#3584",
    "country":"DE",
    "locale":"de-DE"
}
```

### Wallet Object

| Field | Type | Description |
| --- | --- | --- |
| `coinAmount` | Integer | Current coin amount in the user wallet |

```json
{
    "coinAmount":30
}
```

### Round Object

| Field | Type | Description |
| --- | --- | --- |
| `roundId` | String | Round id |
| `status` | String | Round status |
| `betAmount` | Integer | Total bet amount in the round |
| `winAmount` | Integer `[Optional]` | Win amount from the round |

```json
{
    "roundId":"184d4cb3-47b4-417a-84d2-b17392f026b3",
    "status":"pending",
    "betAmount":2
}
```

## API Calls

> [!NOTE]
> For all drop slot calls the subject of the auth token is the session id.

### Drop Slot Balance

Return the current balance and user data.

#### Request

```
GET ${API-BASE-URI}/connect/drop-slot/balance/v1
```

#### Response

| Field | Type | Description |
| --- | --- | --- |
| `r` | Boolean | Response status |
| `data` | Object | Balance data on success |
| `data.user` | Object | User object |
| `data.wallet` | Object | Wallet object |
| `error` | Object | Error details or `null` |

```json
{
    "r":true,
    "data":{
        "user":{
            "userId":"0d5a9c6e-7439-4680-9c2a-145b5dbe28ac",
            "username":"DefensiveBeige#3584",
            "country":"DE",
            "locale":"de-DE"
        },
        "wallet":{
            "coinAmount":30
        }
    }
}
```

### Drop Slot Bet

Subtract coins from the user wallet for a bet. Start of a new round or continue of running/pending round

#### Request

```
POST ${API-BASE-URI}/connect/drop-slot/bet/v1
{
    "betAmount": 1,
    "roundId": "184d4cb3-47b4-417a-84d2-b17392f026b3"
}
```

| Field | Type | Description |
| --- | --- | --- |
| `betAmount` | Integer | Coin amount of the bet. Could be `0` in case of a round with freespins |
| `roundId` | String `[Optional]` | Id of round. If not set, then a new round id will generated. If you want set multiple bets for one round, then use the same round id |

#### Response

| Field | Type | Description |
| --- | --- | --- |
| `r` | Boolean | Response status |
| `data` | Object | Balance data on success |
| `data.wallet` | Object | Wallet object |
| `data.round` | Object | Round object |
| `error` | Object | Error details or `null` |

```json
{
    "r":true,
    "data":{
        "wallet":{
            "coinAmount":28
        },
        "round":{
            "roundId":"184d4cb3-47b4-417a-84d2-b17392f026b3",
            "status":"pending",
            "betAmount":2
        }
    }
}
```

### Drop Slot Close

Close a pending round. Optional it could be booked winning coins to the user.

#### Request

```
POST ${API-BASE-URI}/connect/drop-slot/close/v1
{
    "roundId": "184d4cb3-47b4-417a-84d2-b17392f026b3"
}
```

| Field | Type | Description |
| --- | --- | --- |
| `roundId` | String | Id of round |
| `winAmount` | Integer `[Optional]` | Coin amount, that the user has win |

#### Response

| Field | Type | Description |
| --- | --- | --- |
| `r` | Boolean | Response status |
| `data` | Object | Balance data on success |
| `data.wallet` | Object | Wallet object |
| `data.round` | Object | Round object |
| `error` | Object | Error details or `null` |

```json
{
    "r":true,
    "data":{
        "wallet":{
            "coinAmount":28
        },
        "round":{
            "roundId":"184d4cb3-47b4-417a-84d2-b17392f026b3",
            "status":"closed",
            "betAmount":2,
            "winAmount":0
        }
    }
}
```

### Drop Slot Drop Item

Book a item for the user and close the pending round.

#### Request

```
POST ${API-BASE-URI}/connect/drop-slot/drop-item/v1
{
    "roundId": "184d4cb3-47b4-417a-84d2-b17392f026b3"
}
```

| Field | Type | Description |
| --- | --- | --- |
| `roundId` | String | Id of round |

#### Response

| Field | Type | Description |
| --- | --- | --- |
| `r` | Boolean | Response status |
| `data` | Object | Balance data on success |
| `data.wallet` | Object | Wallet object |
| `data.round` | Object | Round object |
| `error` | Object | Error details or `null` |

```json
{
    "r":true,
    "data":{
        "wallet":{
            "coinAmount":28
        },
        "round":{
            "roundId":"184d4cb3-47b4-417a-84d2-b17392f026b3",
            "status":"closed",
            "betAmount":2,
            "winAmount":0
        }
    }
}
```

### Drop Slot Revert

Revert a running/pending round and booked all betted coins back to the user. The round will be closed.

> [!IMPORTANT]
> A closed round can not revert. Only a round in status `pending`.

#### Request

```
POST ${API-BASE-URI}/connect/drop-slot/revert/v1
{
    "roundId": "184d4cb3-47b4-417a-84d2-b17392f026b3"
}
```

| Field | Type | Description |
| --- | --- | --- |
| `roundId` | String | Id of round |

#### Response

| Field | Type | Description |
| --- | --- | --- |
| `r` | Boolean | Response status |
| `data` | Object | Balance data on success |
| `data.wallet` | Object | Wallet object |
| `data.round` | Object | Round object |
| `error` | Object | Error details or `null` |

```json
{
    "r":true,
    "data":{
        "wallet":{
            "coinAmount":30
        },
        "round":{
            "roundId":"184d4cb3-47b4-417a-84d2-b17392f026b3",
            "status":"reverted",
            "betAmount":2,
            "winAmount":2
        }
    }
}
```
