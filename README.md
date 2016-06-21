# Checkr OAuth Integration Guide

With Checkr OAuth, your customers can easily connect their Checkr account with your application.

OAuth 2 is a protocol that lets you have access to your end user's Checkr account data once they have authorized your application, without knowing their API key or password.

Before getting started, please email clients@checkr.com to register your application.

Every Checkr partner will have its own OAuth application associated with a unique `client_id` and `client_secret` which will be used in the OAuth flow. The `client_secret` should not be shared.

## 1. Flow
When an end user wants to connect their Checkr account to your platform, they’ll go through these steps:

1. On your website, the end user will click a link that takes them to Checkr, passing along your application `client_id`.
2. Once on Checkr’s website, the end user will be prompted to authorize your application by either:
    - Creating a new account with a credit card
    - Logging into their existing account
    - If they are already connected, simply authorizing the application
3. The end user will then be redirected back to your site (specifically to your `redirect_uri`), passing along an authorization `code`.
4. Using this `code`, your application will be able to create an `access_token` for the end user and store it on your data store.
5. Subsequent API requests can be made on behalf of the authorized account using the `access_token` in place of the `API_KEY` or `TEST_API_KEY` typically used for Checkr API requests.

## 2. Integration setup

To get started with your integration, you’ll first need to know two key pieces of information from your platform settings:

- Your `client_id`, a unique identifier for your platform, generated by Checkr.
- Your `client_secret`, a unique secret for your platform, generated by Checkr.

When you register your application with clients@checkr.com, you will need to provide an application `name`, a `description`, an `url`, and a `redirect_uri`. The host of the `url` must match the host from which your application will authenticate end users. Your `redirect_uri` is a page on your website to which the user will be redirected after connecting their account (or failing to, should that be the case). The `redirect_uri` must follow the HTTPS protocol. You may optionally provide a `logo_url` and a `deauthorization_webhook_url`.

The `client_id` and `client_secret` will be provided to you at this time.

### 2.1 Test OAuth integration

In order to create an OAuth integration in test mode, a separate test Application must be created. Please reach out to clients@checkr.com to do so. Using the test `client_id` and test `client_secret` provided by Checkr will return a test `access_token` that will create test resources.

In test mode, a credit card will not be created nor saved when a end user signs up for Checkr and authenticates your application.

Test SSNs and driver licenses are available for use and are detailed in [the Checkr API documentation](https://docs.checkr.com/#development).

## 3. Integration
### 3.1 End user authorization

With these pieces of information on hand, you’re ready to have your end users connect to Checkr within your application. We recommend showing a Connect button that sends them to our authorize url endpoint:

```
https://dashboard.checkr.com/oauth/authorize/:client_id?scope=read_write
```

To prevent CSRF attacks, you can also use the `state` parameter, passing along a unique token as the value. We’ll include the `state` you gave us when we redirect back.

Here is how this may look:

![Connect with Checkr](https://assets.checkr.com/assets/images/connect_with_checkr.png)


### 3.2 Token issuance

When the end user arrives at Checkr, they’ll be prompted to allow or deny the connection to your platform, and will then be sent to your `redirect_uri` page. In the URL, we’ll pass along an authorization code:

```
https://YOUR-REDIRECT-URI.com/REDIRECT-ENDPOINT?scope=read_write&code=AUTHORIZATION_CODE
```

Using the `code` parameter, you should make a POST request to our tokens endpoint to retrieve the `access_token`:

```curl
curl -X POST https://api.checkr.com/oauth/tokens \
    -d client_id=YOUR_CLIENT_ID \
    -d client_secret=YOUR_CLIENT_SECRET \
    -d code=AUTHORIZATION_CODE
```

Note that you’ll make the request with your live secret API key or test secret API key, depending on whether you want to get a live or test `access_token` back.

Checkr will return a response containing the authentication credentials for the user:

```json
{
  "access_token": "xoxt-23984754863-2348975623103",
  "scope": "read_write",
  "checkr_account_id": "XXXX"
}
```

You’re done! The user has now authorized your platform.  You’ll want to store all of the returned information in your database for later use.

### 3.3 Make API calls using user tokens

You can now make requests to the [Checkr API](https://docs.checkr.com) using the `access_token` from the previous step.

```curl
curl -X GET https://api.checkr.com/v1/candidates \
    -u END_USER_ACCESS_TOKEN:
```

### 3.4 User deauthorization

#### 3.4.1 From Checkr's dashboard
A `token.deauthorized` webhook event is sent when an end user disconnects your platform from their account. By watching for this event, you can perform any necessary credential cleanup on your servers. Note that a managed account cannot be disconnected by the account’s beneficiary.

#### 3.4.2 From your system

Additionally, if you want to disconnect access to an account, you can POST to `https://api.checkr.com/oauth/deauthorize`:

```curl
curl https://api.checkr.com/oauth/deauthorize \
   -u END_USER_ACCESS_TOKEN:
```

## 4. Additional per-account configuration steps
### 4.1 Webhooks
You may want to start listening Webhooks events (eg: `report.completed`) for your authorized accounts. To do so, you will need to manage Webhooks for every account.

```curl
curl -X POST https://api.checkr.com/v1/webhooks \
  -d webhook_url=https://YOUR-APPLICATION.COM/END-USER-CHECKR-ENDPOINT \
  -d region=us \
  -u END_USER_ACCESS_TOKEN:
```

See the [Checkr Webhooks API](https://github.com/checkr/oauth-docs/blob/master/checkr_webhooks_api.md) for a complete guide.

### 4.2 Packages
You may want to have a dynamic way of creating and listing a report's package on a per-account basis. To add packages, you will need to manage `Package`s of every end user's authorized account by making requests to our Packages API using the end user's `access_token`. If you are using a test `client_id` and test `client_secret`, we will automatically create test default packages for an end user's account. The default packages are `tasker_standard`, `tasker_pro`, `driver_standard`, and `driver_pro`. See more details about the package types [here](https://docs.checkr.com/#report).

See the [Checkr Packages API](https://github.com/checkr/oauth-docs/blob/master/checkr_packages_api.md) for a complete guide.

## 5. Checkr OAuth full API Reference

See the [Checkr OAuth Full Reference](https://github.com/checkr/oauth-docs/blob/master/checkr_oauth_full_reference.md) for a complete guide on the Checkr OAuth API.
