# Checkr Packages API

The package API and package UI allow customers and partners to create and manage as many custom packages as they need.

Authentication to the API occurs via HTTP Basic Auth, with either the end user's access token (for Checkr partners), your API key, or your test API key as the username and the password set to null.

### Attributes

```json
{
    "slug": "employee_finance",
    "name": "Employee Finance",
    "apply_url": "https://checkr.com/apply/checkr/xif2821opw",
    "screenings": [
        {
            "type": "ssn_trace",
        },
        {
            "type": "national_criminal_search",
            "subtype": null
        },
        {
          "type": "county_criminal_search",
          "subtype": "7years"
        }
    ]
}
```

### Screenings

Here is a list of all screenings available and their subtypes if any:

| Screening type| Subtypes available |
| ------------- |:-------------:|
|ssn_trace||
|sex_offender_ search||
|motor_vehicle_report||
|national_criminal_search||
|county_criminal_search|'current', '7years'|
|county_civil_search|'current', '7years'|
|federal_criminal_search||
|federal_civil_search||
|employment_verification|'current', 'last3'|
|education_verification||
|eviction_search||

### POST /v1/packages

Create a new package.

Here are the params accepted:

| Name| Required?  | Type | Description |
| ------------- |:-------------:| -----:| -----:|
| slug | true | string |  Package slug |
| name | true | string | Package name |
| screenings | true | array | Array of Screenings |


### GET /v1/packages

List all packages.
This is a paginated endpoint (see [#pagination](https://docs.checkr.com/#pagination)).

Example of response:

```json
{
    "data": [
        {
            "id": "887445e43144af09b3ac9f70",
            "object": "test_package",
            "slug": "tasker_standard",
            "name": "Tasker Standard",
            "apply_url": "https://checkr.com/apply/try-checkr/19ceb9b3d5bb",
            "screenings": [
                {
                    "type": "county_criminal_search",
                    "subtype": "current"
                },
                {
                    "type": "national_criminal_search",
                    "subtype": null
                },
                {
                    "type": "sex_offender_search",
                    "subtype": null
                },
                {
                    "type": "ssn_trace",
                    "subtype": null
                },
                {
                    "type": "terrorist_watchlist_search",
                    "subtype": null
                }
            ],
            "uri": "/v1/packages/tasker_standard",
            "created_at": "2015-08-14T21:08:11Z",
            "deleted_at": null
        },
        {
            "id": "da942b621515e2fdce06e4f9",
            "object": "test_package",
            "slug": "driver_standard",
            "name": "Driver Standard",
            "apply_url": "https://checkr.com/apply/try-checkr/5ddfb31089c2",
            "screenings": [
                {
                    "type": "county_criminal_search",
                    "subtype": "current"
                },
                {
                    "type": "motor_vehicle_report",
                    "subtype": null
                },
                {
                    "type": "national_criminal_search",
                    "subtype": null
                },
                {
                    "type": "sex_offender_search",
                    "subtype": null
                },
                {
                    "type": "ssn_trace",
                    "subtype": null
                },
                {
                    "type": "terrorist_watchlist_search",
                    "subtype": null
                }
            ],
            "uri": "/v1/packages/driver_standard",
            "created_at": "2015-08-14T21:08:11Z",
            "deleted_at": null
        },
        {
            "id": "ffd159b59de63e695bad00b8",
            "object": "test_package",
            "slug": "tasker_pro",
            "name": "Tasker Pro",
            "apply_url": "https://checkr.com/apply/try-checkr/e8cd0ab39f79",
            "screenings": [
                {
                    "type": "county_criminal_search",
                    "subtype": "7years"
                },
                {
                    "type": "national_criminal_search",
                    "subtype": null
                },
                {
                    "type": "sex_offender_search",
                    "subtype": null
                },
                {
                    "type": "ssn_trace",
                    "subtype": null
                },
                {
                    "type": "terrorist_watchlist_search",
                    "subtype": null
                }
            ],
            "uri": "/v1/packages/tasker_pro",
            "created_at": "2015-08-14T21:08:11Z",
            "deleted_at": null
        }
    ],
    "object": "list",
    "next_href": null,
    "previous_href": null,
    "count": 9
}
```

### GET /v1/packages/:slug

Retrive one single Package.

Example of response:

```json
{
    "id": "887445e43144af09b3ac9f70",
    "object": "test_package",
    "slug": "tasker_standard",
    "name": "Tasker Standard",
    "apply_url": "https://checkr.com/apply/try-checkr/19ceb9b3d5bb",
    "screenings": [
        {
            "type": "county_criminal_search",
            "subtype": "current"
        },
        {
            "type": "national_criminal_search",
            "subtype": null
        },
        {
            "type": "sex_offender_search",
            "subtype": null
        },
        {
            "type": "ssn_trace",
            "subtype": null
        },
        {
            "type": "terrorist_watchlist_search",
            "subtype": null
        }
    ],
    "uri": "/v1/packages/tasker_standard",
    "created_at": "2015-08-14T21:08:11Z",
    "deleted_at": null
}
```
