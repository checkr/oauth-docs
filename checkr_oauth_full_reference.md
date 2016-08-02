# Checkr OAuth Full Reference

### Authorization

#### GET https://dashboard.checkr.com/oauth/authorize/:client_id

| Name| Required?  | Description |
| ------------- |:-------------:| :-----:|
| redirect_uri | optional- domain must match redirect_uri set during application registration and must use https protocol | URL to redirect back to |
| scope | optional (default: `read_write`, possible values: `read_write`) | Permissions to request |
| state | optional | Unique string to be passed back upon completion |

#### GET https://YOUR-REDIRECT-URI.com

| Name | Description |
| :------------- |:-------------:| :-----:|
| code | An authorization code you can use in the next call to get an access token for your user. This can only be used once and expires in 5 minutes.|
| state | The value of the state parameter you provided on the initial GET request. |
| error | Error code, if any error. |

### Token Issuance

#### POST https://api.checkr.com/oauth/tokens

| Name| Required? | Type | Description |
| ------------- |:-------------:| :-----:| :-----:|
| client_id | required | string | Issued when you created your application|
| client_secret | required | string |Issued when you created your application|
| code | required | string | The value of the code returned previously |

Request:
``` curl
curl -X POST https://api.checkr.com/oauth/tokens \
    -d client_id=XXX \
    -d client_secret=XXX \
    -d code=XXX
```

Response:
```json
{
    "access_token": "xoxt-23984754863-2348975623103",
    "scope": "read_write",
    "checkr_account_id": "XXXX"
}
```

### Deauthorize

#### POST https://api.checkr.com/oauth/deauthorize

Request:
``` curl
curl -X POST https://api.checkr.com/oauth/deauthorize \
    -u END_USER_ACCESS_TOKEN:
```

Response:
```json
{
  "access_token": "xoxt-23984754863-2348975623103"
}
```

### Deauthorize callback

#### POST https://YOUR-DEAUTHORIZATION-CALLBACK.com

```json
{
  "id": "5612db68303137000a311000",
  "object": "event",
  "type": "token.deauthorized",
  "created_at": "2015-10-05T20:19:52Z",
  "data": {
    "object": {
      "access_code": "XXXXXXX"
    }
  }
}
```
