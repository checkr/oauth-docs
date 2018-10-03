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
  "id": "5ba02cd2079784004bd67ba5",
  "object": "event",
  "type": "account.credentialed",
  "created_at": "2018-09-17T22:38:10Z",
  "data": {
    "object": {
      "id": "credentialed integrated account id",
      "adjudication_filters_enabled": false,
      "adverse_action_email": null,
      "adverse_action_enabled": true,
      "adverse_action_notice_days": 7,
      "api_authorized": true,
      "apply_canada_uris": null,
      "authorized": true,
      "better_future_banner": false,
      "billing_email": "jeff@yourcustomer.com",
      "canada_enabled": false,
      "continuous_monitoring": false,
      "candidates_view_enabled": false,
      "credit_report_reasons": [],
      "credit_report_setting": false,
      "default_compliance_city": "San Diego",
      "default_compliance_state": "CA",
      "free_credits": 5,
      "geos_required": null,
      "in_house_adjudication": false,
      "individualized_assessment": false,
      "invitation_period": 7,
      "manual_orders_enabled": false,
      "name": "Your Customer Co.",
      "notify_assessment_changed": true,
      "notify_candidate_report_started": false,
      "pilot_account": false,
      "programs_enabled": false,
      "request_aliases_hard_cap_threshold": null,
      "requires_education_history_enabled": false,
      "requires_employment_history_enabled": false,
      "self_serve_packages_enabled": false,
      "self_service_applications": true,
      "show_adjudication_filtered_charges": true,
      "skip_email_on_id_and_dl_exceptions": false,
      "support_email": null,
      "support_phone": null,
      "technical_contact_email": "jill@yourcustomer.com",
      "compliance_contact_email": null,
      "uri_name": "yourcustomer",
      "logo_uri": null,
      "available_screenings": [
        "county_civil_search",
        "county_criminal_search",
        "..."
      ],
      "purpose": "employment",
      "webhook_setting": {
        "candidate_created": false,
        "candidate_driver_license_required": false,
        "candidate_engaged": false,
        "candidate_updated": false,
        "invitation_completed": true,
        "invitation_created": true,
        "invitation_expired": true,
        "report_completed": true,
        "report_created": true,
        "report_disputed": true,
        "report_resumed": true,
        "report_suspended": true,
        "report_updated": false,
        "report_upgraded": true
      }
    }
  }
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
  }
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
