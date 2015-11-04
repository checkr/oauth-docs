# Checkr Webhooks API

Authentication to the API occurs via HTTP Basic Auth, with either the end user's access token (for Checkr partners), your API key, or your test API key as the username and the password set to null.

### Attributes
```json
{
    "id": "cc0d35de9feda638a7704149",
    "object": "webhook",
    "uri": "/v1/webhooks/a235ed94f01d431a083da677",
    "region":"us",
    "webhook_url": "https://your-app.com/incoming/checkr",
    "deleted_at": null,
    "created_at": "2014-01-18T12:34:00Z"
}
```

### POST /v1/webhooks

Post a new webhook.

Here are the params accepted:

| Name| Required?  | Type | Description |
| ------------- |:-------------:| :---------: | :-----|
| url | true | string | URL where webhooks should be sent. Should start with `http` or `https`. |
| region| true | string | `us` or `ca` |

```curl
curl -X POST https://api.checkr.com/v1/webhooks \
  -d webhook_url=https://YOUR-APPLICATION.COM/END-USER-CHECKR-ENDPOINT \
  -d region=us \
  -u END_USER_ACCESS_TOKEN:
```

### GET /v1/webhooks/:id

| Name| Required?  | Description |
| ------------- |:-------------:| -----:| -----:|
| id | true | ID of the Webhook |

### GET /v1/webhooks

### DELETE /v1/webhooks/:id

| Name| Required?  | Description |
| ------------- |:-------------:| -----:| -----:|
| id | true | ID of the Webhook |
