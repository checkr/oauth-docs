# Checkr OAuth Webhooks

This guide will explain OAuth integration specific webhooks.

## `account.credentialed`

### When we send them:

When an end user authorizes your platform to manage resources on its behalf, Checkr will check that the account is credentialed to send background checks.

If an integrated account is already credentialed, we will send the webhook as soon as a token is generated for the integrated account. If not, Checkr's Customer Success team will reach out to the end user tied to the integrated account to provide additional documents. When that process is complete, we will send a webhook.

### Who do we send them to:

This webhook will only go to you, the partner. It will be sent to the endpoint specified in the `webhook_url` specified during registration. Please reach out to `clients@checkr.com` if you need to adjust this endpoint.

### What it looks like:

```json
{
  "id":"57a10c5d4772615f58010000",
  "object":"event",
  "type":"account.credentialed",
  "created_at":"2016-08-02T21:10:53Z",
  "data":{
    "object":{
      "account":{
        "id":"credentialed integrated account id"
      }
    }
  },
  "account_id":"account id tied to your platform"
}

```


## `token.deauthorized`


### When we send them:

An integrated account can deauthorize your platform from managing its resources via the Checkr dashboard. Additionally, you can also disconnect access to an integrated account through our public API. See [Checkr OAuth Full Reference](https://github.com/checkr/oauth-docs/blob/master/checkr_oauth_full_reference.md) for more details.

### Who do we send them to:

This webhook will only go to you, the partner. It will be sent to the endpoint specified in the `webhook_url` specified during registration. Please reach out to `clients@checkr.com` if you need to adjust this endpoint.

### What it looks like:

```json
{
  "id":"57a10deb4772616473010000",
  "object":"event",
  "type":"token.deauthorized",
  "created_at":"2016-08-02T21:17:31Z",
  "data":{
    "object":{
      "access_code":"dummy access token"
    }
  },
  "account_id":"5d78dfa52ea938723b2f2ba3"
}
```

## General webhooks

Checkr sends webhooks for events as detailed in the [Checkr API documentation](https://docs.checkr.com/#webhooks). Some common examples are:
- `report.created`
- `candidate.created`
- `report.completed`
- `report.upgraded`

### When we send them:

Each webhook is sent when the event associated with it occurs.

### Who do we send them to:

These webhooks will be sent to the endpoint specified in the `webhook_url` during registration. In addition, they will be sent to any webhooks belonging to the integrated account, if any.

### What it looks like:

```json
{
  "id":"57a1142c4772617d5c010000",
  "object":"event",
  "type":"report.created",
  "created_at":"2016-08-02T21:44:12Z",
  "data":{
    "object":{
      "id":"f4bda8bd0201d322e41301aa",
      "object":"report",
      "uri":"/v1/reports/f4bda8bd0201d322e41301aa",
      "status":"pending",
      "created_at":"2015-10-15T23:09:44Z",
      "completed_at":null,
      "upgraded_at":null,
      "turnaround_time":null,
      "due_time":"2014-01-19T11:28:31Z",
      "adjudication":"engaged",
      "package":"tasker_standard",
      "tags":[],
      "candidate_id":"3cd3d2bf54acfd7207caa276",
      "ssn_trace_id":null,
      "sex_offender_search_id":null,
      "terrorist_watchlist_search_id":null,
      "national_criminal_search_id":null,
      "state_criminal_search_ids":[],
      "federal_criminal_search_id":null,
      "federal_civil_search_id":null,
      "county_criminal_search_ids":[],
      "motor_vehicle_report_id":null,
      "personal_reference_verification_ids":[],
      "professional_reference_verification_ids":[],
      "document_ids":[]
    }
  },
  "account_id":"5d78dfa52ea938723b2f2ba3"
}
```
