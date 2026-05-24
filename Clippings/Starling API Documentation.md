---
title: "API Documentation"
source: "https://developer.starlingbank.com/docs"
author:
published:
created: 2026-05-23
description: "Explore the Starling API - Use the in-depth documentation, sandbox environment and SDK to build exciting integrations with the bank."
tags:
  - "clippings"
---
## Starling Bank API

This page describes the developer APIs we provide for Starling Bank account holders and regulated third party providers. If you are a payments business looking for our Payment Services APIs, you’ll find them [here](https://developer.starlingbank.com/payments/docs). If you are looking for our OBIE AISP and PISP docs, you can find them [here](https://developer.starlingbank.com/docs/open-banking).

### Introduction

The Starling Public API enables seamless integration of Starling Bank account and transactional data into your application. You can, subject to permission, use it to view account holder data and to automate actions on accounts.

Our API is RESTful, using predictable and resource-orientated URLs with standard HTTP methods and status codes. All responses (expected and errors) are in JSON format. We also offer webhooks so our API can notify you of events.

All API requests are authorised using [OAuth2](http://oauth.net/2/) with different permission scopes required depending on the services you wish to use. This allows our account holders to see the data your application can access and allows us to ensure only trusted partners can carry out significant actions, such as transferring money on our account holder's behalf.

We want to make integrating with Starling as easy as possible. If you have feedback on our APIs or documentation or there is something you wish to see covered, please get in touch through the [Starling Developer Slack](https://developer.starlingbank.com/community) or by [email](mailto:developer@starlingbank.com).

[Back to top](#on-this-page-1)

### What's new

#### Changes to Strong Customer Authentication (SCA)

On 26th March 2022, the FCA published an exemption to its requirements for SCA, under new Article 10a. Previously, we required customers to reconfirm their consent to data sharing every 90 days. Under Article 10a, data providers are exempted from applying SCA every 90 days as long as the Payment Services User (PSU) re-confirms their consent with the Third Party Provider (TPP) at least every 90 days. Furthermore, we must restrict the data available to only account balances and/or transactions executed in the last 90 days.

In line with the above, we have implemented the following technical changes:

- We no longer prompt customers to re-authorise API connections every 90 days. This is instead the responsibility of the TPP.
- The consent expiry date indicated by the token identity endpoint transitions from a "hard" expiry to a "soft" expiry. After the consent has expired, the API will still accept existing access tokens as long as the PSU continues to provide the consent directly with the TPP. The data available in this scenario will be significantly restricted.
- After soft expiry of the consent, we will only provide data pertaining to current account balances and transactions executed within the last 90 days.
	- Any attempt to retrieve details of a specific transaction which is more than 90 days ago, we will return a HTTP 403 response.
		- Any attempt to retrieve a range of transactions which span the 90 day threshold will be filtered to include only those within the 90 day scope.
		- Any attempt to retrieve any other data point which falls outside the exemption will also receive a HTTP 403 response.
- TPPs can continue to exchange refresh tokens for new access tokens after the consent has expired.

#### Rate Limiting

To allow us to continue to provide a high level of service to our API consumers, we have introduced client rate limiting on our API. As is customary, we will return a HTTP 429 response with a Retry-After header whenever we reject a request due to a breach of the rate limit. We reserve the right to amend these limits as required.

Initially we will apply the following limits across all of our API endpoints.

- TPPs are given a flat limit of 100 requests per second per client\_id.
- Personal access tokens have significantly more restrictive limits.
	- 5 requests per second
		- 1000 requests per day

[Back to top](#on-this-page-1)

### The basics

#### API access

Our APIs are open to everyone to develop and test applications. There are different levels of access:

- Using a personal access token. This allows you to use our public APIs to access your own personal and business accounts.
- Using our sandbox to access dummy data. Here, you can experiment with our public APIs without needing to be an approved provider.
- As a regulated [Third Party Provider](https://www.openbanking.org.uk/providers/third-party-providers/) (TPP). Depending on your regulatory approval you have, you can use our AISP and/or PISP endpoints to access any Starling Bank account, provided you have the account holder's permission.
- As a Marketplace provider. In this case, we will consider the specific access you require, subject to our due diligence process. Of course, you also need the Starling Bank account holder's permission.

[Back to top](#on-this-page-1)

#### The Developer Portal

The Starling Developer Portal allows you to:

- register and configure your application
- create webhooks to receive notifications about events
- upload API keys to secure calls to API endpoints that require message signing for additional security
- use our Sandbox Simulator to create test customers and accounts, and populate the accounts with transactions
- create a personal access token to access your own Starling bank account

[Back to top](#on-this-page-1)

#### Security

We use secure server-to-server communications, protected by message signing using public and private key pairs.

We use [OAuth2](http://oauth.net/2/) authorisation to provide secure access to our APIs and hence to customer account data. We use OAuth scopes to control access to our endpoints: even if you have been authenticated to use our APIs, you can only access the endpoints for which you have the access scopes.

We use [message signing](#message-signing-1) with digital signatures for the endpoints that require additional security.

We also recommend that you use [SSL pinning](#ssl-pinning-1) in production applications for client-side security to avoid man-in-the-middle attacks.

[Back to top](#on-this-page-1)

#### Authentication and authorisation

**Third party providers**

If you are a third party provider, first, you must register your application in our Developer Portal.

You receive a client ID and client secret which you use later to identify yourself to us. We grant your application the access scopes you need to access customer account data. The scopes you receive depend on your regulatory approval and our due diligence process.

Before you can access customer data, your application must follow our authorisation flow so the customer can authorise your access to their data. When the customer has authorised you to access their data, we give you an authentication code, which you exchange for an access token and refresh token, specific to that customer. You supply the access token when you call our API endpoints.

Access tokens are short-lived (typically valid for an hour or so) and refresh tokens are much longer-lived (typically valid for several months). When the access token gets close to expiry, you call our OAuth token refresh endpoint, passing the refresh token, to obtain a new access token and refresh token. Note only one access token per customer and registered application can be active at a given time.

You can read more about our OAuth flow [here](#oauth-flow-1).

**Personal access tokens**

If you just want a personal access token to access your own Starling Bank account data, you can link your account and create a personal access token in the Developer Portal. You can select the access scopes you require from the range of scopes available to you.

You don't need to go through our OAuth process to obtain the access token. The token doesn't expire, so doesn't need to be refreshed.

[Back to top](#on-this-page-1)

#### Access tokens and access scopes

Each access token grants access to a single customer account and has one or more OAuth scopes associated with it. The Oauth scopes control access to our API endpoints: to call an endpoint you need to supply a current access token with the scope required by that endpoint.

So, for example, in order to call our endpoint `GET /api/v2/accounts/{accountUid}/balance` to read a customer account balance, you would need a valid access token for that account with the `balance:read` OAuth scope.

To [access your own Starling Bank account](#accessing-your-own-starling-bank-account-1), you create a personal access token in the Developer Portal. You can select the OAuth scopes you need from the available scopes.

If you are a regulated TPP, the scopes you receive depend on the regulatory approval you have.

Each [API endpoint specification below](#api-reference-1) includes the Oauth scope required by the endpoint.

[Back to top](#on-this-page-1)

#### Message signing

Some API endpoints use message signing for additional security (for example, for making payments).

To use message signing, you create an API key pair, and upload the public key of the pair to our Developer Portal. You then use the private API key to sign API requests. We use the corresponding public key to verify that the message originated from you. If we receive a properly signed request, we attempt to execute that request.

You can read more about API keys and message signing [here](#api-keys-and-message-signing-1).

[Back to top](#on-this-page-1)

#### Webhooks

Webhooks allow you to register for feed item and standing order events.

You register for webhook events in the Developer Portal. When an event occurs that you have registered for, we send a notification to the endpoint URL you specified.

You can read more about webhooks [here](#v2-webhooks-1).

[Back to top](#on-this-page-1)

### Developing applications

#### The Developer Portal account

To develop your own applications, you need to [sign up for a developer account](https://developer.starlingbank.com/signup) in our Developer Portal. Here, you can:

- create personal access tokens to access your own Starling bank account
- register applications
- use our sandbox to test registered applications against dummy data (no approvals needed)
- use registered applications to access live Starling bank accounts other than your own (regulated and approved third parties only)

[Back to top](#on-this-page-1)

#### Accessing your own Starling Bank account

You can create a personal access token to access your own Starling Bank account.

To create a personal access token:

1. [Log in](https://developer.starlingbank.com/login) to the Developer Portal.
2. Go to the [Personal Access page](https://developer.starlingbank.com/personal).
3. Link your Starling Bank account to your Starling Developer account.
4. Create a token, selecting the scopes you require. Note that you can initiate payments from your account via our API, but only to existing payees.
5. Use the personal access token to make API requests against your own Starling Bank account.

Keep your personal access token secure, and never give it to a third party or embed it in publicly available code. Anyone who has it can access to your bank account, so treat it as you would your username and password.

You can only link a Developer Portal account to one Starling Bank account. If you want to link more than one Starling Bank account (for example, a personal account and a business account) you need a separate Developer Portal account for each Starling Bank account you want to link.

[Back to top](#on-this-page-1)

To use our APIs to access Starling Bank accounts other than your own, you must [register an application](https://developer.starlingbank.com/application) in the Developer Portal. This gives you a unique `client_id` and `client_secret`. You use these in our [OAuth flow](#oauth-flow-1) to get access tokens for customers' Starling Bank accounts.

If you have an entry in the [Open Banking Directory](https://directory.openbanking.org.uk/s/login/), you can use your SSA (Software Statement Assertion) to create an application [here](https://developer.starlingbank.com/application/new). Because the SSA proves your service provider status based on the information in the directory, you can select the Scopes that you will require as you create the application. This is quicker than adding scopes after creating an application, as you do not have to wait for approval this way (See: [Access tokens and access scopes](#access-tokens-and-access-scopes-1)).

Once you have registered an application, you can take it into our sandbox and try it out against dummy data without needing any special permissions or authorisation. If you are a regulated third party provider or an approved Marketplace provider, you can also apply for authorisation to access live accounts.

[Back to top](#on-this-page-1)

#### The sandbox

Our sandbox environment gives you the chance to thoroughly test your application against our API. You can also use it to experiment with our APIs to see what they can do before seeking approval for production access.

To use our Sandbox, register your application as usual in the Developer Portal, then take it into the [sandbox](https://developer.starlingbank.com/sandbox). Here you can:

- create test customers and accounts
- simulate card, FPS and BACS transactions
- get access tokens and refresh tokens to use with our APIs to access sandbox account data

Sandbox access tokens (like production access tokens) are issued on a per-customer basis. They are longer-lived than production access tokens so do not need to be refreshed as frequently. You can either obtain and refresh the tokens programmatically using our OAuth endpoints (as a production application would do) or you can do this manually in the sandbox.

[Back to top](#on-this-page-1)

#### Accessing live Starling Bank accounts

You cannot access live Starling Bank accounts (other than your own) unless you are a regulated third party provider (either an AISP or a PISP), or an approved Marketplace provider.

Regulated third party providers need an OBIE certificate (UK-based providers) or an eIDAS certificate (EU-based providers). We can accept either an OBWAC or OBSeal OBIE certificate or a qualified QWAC or QSeal eIDAS certificate. The certificate should have roles that cover the [permissions](https://developer.starlingbank.com/permissions) required by your application.

Please [contact us](mailto:developer@starlingbank.com?subject=UK-based%20production%20access%20request) so we can approve your application and grant you the scopes you are entitled to.

[Back to top](#on-this-page-1)

#### Security requirements

To protect account holder data, your application:

- **must** keep the production `client_secret` confidential on the production server, and **must not** send it to the browser
- **must** maintain a secure production environment with limited and monitored access
- **must** hold production access tokens and refresh tokens in a secure store with limited and monitored access
- **must** use HTTPS resources for receiving the authorisation code redirect and for displaying Starling data
- **must** conform with relevant national and international data and personal information protection laws

[Back to top](#on-this-page-1)

#### SSL pinning

SSL Pinning is a client-side security technique to avoid man-in-the-middle attacks by manually re-validating the certificate. We highly recommend the use of SSL pinning in production.

The SSL handshake verifies the certificate provided by the server is a specific, pre-agreed certificate. This prevents data loss in the unlikely event that an attacker hijacks our API URL and uses a different valid certificate (signed by a trusted CA) to pose as us.

You can download our X509 public certificates here:

- [Production Certificate](https://developer.starlingbank.com/starling-prod-api-certificate.crt)
- [Sandbox Certificate](https://developer.starlingbank.com/starling-sandbox-api-certificate.crt)
- [Sandbox Token API Certificate](https://developer.starlingbank.com/starling-sandbox-token-api-certificate.crt)

You need to embed the certificate during development and associate it with our domain. Then, during the SSL challenge with us your application should validate our identity with the certificates. Libraries to do this are available for almost all languages and platforms, for example `TrustKit` on iOS and `okhttp` for Java.

[Back to top](#on-this-page-1)

\`

### Using the API

#### Permissions

Different API endpoints require different permissions to access data. Permissions correspond to OAuth scopes.

You can find the OAuth scopes and permissions on the [permissions information](https://developer.starlingbank.com/permissions) page. Each [API endpoint specification below](#api-reference-1) includes the Oauth scope required by the endpoint.

Access is given to an application - not to a developer account. This means each application you register has its own permissions.

[Back to top](#on-this-page-1)

#### API endpoint URLs

The base URLs for the Starling public API endpoints are:

- Production (to access live Starling Bank accounts, including your own Starling account using a personal access token): `https://api.starlingbank.com`
- Sandbox (to access sandbox accounts): `https://api-sandbox.starlingbank.com`

You can find the API endpoints themselves [below](#api-reference-1).

[Back to top](#on-this-page-1)

#### API methods

The API uses four HTTP methods:

| Method | Usage |
| --- | --- |
| `POST` (create) | Makes a new object, always returns the UUID for the object made. |
| `GET` (read) | Returns details of the requested object. |
| `PUT` (update) | Updates an existing object, usually no response content is returned. |
| `DELETE` (delete) | Deletes an existing object, usually no response content is returned. |

All responses, including errors, return JSON. The `POST` and `PUT` methods require JSON for the request body.

[Back to top](#on-this-page-1)

#### API status codes

We use standard HTTP status codes in the response header, including, but not limited to, the following:

| HTTP Status | Meaning |
| --- | --- |
| `200` - OK | The request succeeded and processed OK. |
| `202` - Accepted | The request has been accepted for processing but processing has not completed. Typically you receive a UUID in the response that you can use to check progress. |
| `204` - No Content | The request succeeded and there is no content to send in the response payload body. |
| `400` - Bad Request | Something was wrong with the request. For more details, check the error message included in the response. |
| `401` - Unauthorized | Your authentication credentials are invalid. |
| `403` - Forbidden | Your authentication failed, probably because the access token has expired, or you tried to access a resource outside the scope of the token. |
| `404` - Not Found | The requested resource does not exist. |
| `5xx` - Server Error | Something went wrong on our side - get in touch so we can look into it. |

[Back to top](#on-this-page-1)

#### Character Encoding

Starling APIs use `UTF-8` character encoding for all responses and headers. For the best experience we recommend that requests use `UTF-8` encoding only.

[Back to top](#on-this-page-1)

### OAuth flow

Starling uses [OAuth2](http://oauth.net/2/) to authenticate and authorise requests to our API. This allows Starling Bank account holders to authorise your application to access their data, without handing over their Starling credentials.

1. You direct the user to Starling (usually via a button in your application).
2. We authenticate the user, then they authorise your application to access their data.
3. We redirect the user back to your application and give you an authorisation code.
4. You exchange the authorisation code for an access token and a refresh token. You present your OBIE or eIDAS certificate to secure this request.
5. You use the Starling API, supplying the access token with every request.
6. Later, you use the refresh token to get a new access token and refresh token. Again, you present your OBIE or eIDAS certificate to secure this request.

> [!warning] Warning
> Note
> 
> Each access token is associated with a single application.
> 
> If the same Starling account holder may be running multiple instances of your application (for example, if you provide a connection to Starling for multiple third party applications) then you must ensure that each instance of your application uses a separate OAuth flow and obtains a separate access token for each account holder.

If you have a Starling Bank account and want to access your own account data, you can create a [personal access token](https://developer.starlingbank.com/personal) in our Developer Portal. Then you can go straight to step 5 and use the Starling API, supplying your personal access token. Personal access tokens don't expire, so you don't need to refresh them.

[Back to top](#on-this-page-1)

#### 1\. Redirecting to Starling

The user starts the authentication process in your application. This may be a web page or a native application, either on the phone with the Starling app or on a separate device.

You redirect the user to the Starling OAuth page:

- Sandbox: `https://oauth-sandbox.starlingbank.com`
- Production: `https://oauth.starlingbank.com`

At this stage, you can optionally specify a redirect URI where we return the user after authentication. If you omit the redirect URI, we use the default redirect URI you provided when you [registered your application](https://developer.starlingbank.com/application) in our Developer Portal. If you provide a specific redirect URI, **it must either match the default redirect URI, or be a sub-resource of it**.

You can also optionally specify the access scopes your application requires. If you omit the `scopes` parameter, we grant you all the scopes that we have previously approved for your application. Since these are the scopes we show to the user when we ask them to authorise access to their data, if you don't need all the scopes you have, you may prefer to only request the scopes you require.

The query parameters are:

| Parameter | Value |
| --- | --- |
| `client_id` | Your client ID for your registered application |
| `response_type` | The type of response you are expecting - this must be `code` |
| `state` | An unpredictable randomised string used to protect against cross-site request forgery ([more details](https://auth0.com/docs/protocols/oauth2/oauth-state)) |
| `redirect_uri` | (Optional) The URI we redirect the user to after they have authorised your application. If omitted, we use the default redirect URI you provided when you registered your application in our Developer Portal.      If supplied, this must either match the default redirect URI, or be a sub-resource of it.      If you supply a `redirect_uri` that is not the default redirect URI or a sub-resource of it, the user sees an error and the OAuth flow terminates. |
| `scope` | (Optional) A space-separated list of the access scopes your application requires (using `%20` to encode each space). You can only include scopes that we have previously approved for your application.      If omitted, we grant you all your approved scopes. |

An example OAuth URI:

```markdown
https://oauth.starlingbank.com/?client_id=aT6TPtsTJvhB1wzGbrRp&response_type=code&state=249a34c4-4588-449a-9ad9-f192581e52fd&redirect_uri=https://your.application.url/redirect/path&scope=account-list:read%20confirmation-of-funds:read
```

[Back to top](#on-this-page-1)

#### 2\. User authentication and authorisation

We authenticate the user and then ask them to authorise your application.

Firstly, we identify and verify the user (authentication) then we ask them for permission to share their data with your application (authorisation). The way we do this depends on whether they are using the device with the Starling app installed, or another device. In both cases, we walk them through the process.

- If the user is on the device with the Starling app, we send an in-app notification telling them the kind of access you are requesting. The user can approve or deny the request. If they approve it, they can choose which accounts you can access.
- If the user is on another device, we open a browser window telling them the kind of access you are requesting, and asking the user for details of the Starling account they want to share with you. We then send a notification to the user's Starling app, where they can approve or deny the request.

If the user approves access, the authorisation flow continues with the next step.

If the user denies access at any stage, we send them to your `redirect_uri` with the `state` you sent us in step 1 and an `error` of `access_denied`.

[Back to top](#on-this-page-1)

#### 3\. Receiving the authorisation code

Next, we return an authorisation code to your redirect URI.

Once we've verified the user's identity and they have authorised your application, we redirect them to your `redirect_uri` with two parameters:

| Parameter | Value |
| --- | --- |
| `code` | A temporary authorisation code you can exchange for an access token to obtain the user's data. The code expires after a few minutes. |
| `state` | The `state` you sent us in step 1. You **must** verify `state` matches the state you sent us, and abort the process if it does not. |

For example, if the `redirect_uri` was `https://your.application.url/redirect/path` the user might be redirected to:

```markdown
https://your.application.url/redirect/path?code=edv79CsZn80frhfxgF4o58aXAc7enpgB1wyv&state=249a34c4-4588-449a-9ad9-f192581e52fd
```

##### Redirecting to a native mobile application

If you are developing a native mobile application, you need to register your application with the OS before sending the user to Starling. This allows the OS to seamlessly return the user into the correct screen of your application when we redirect the user back.

iOS [Universal links](https://developer.apple.com/ios/universal-links/) allow your application to use the same URIs as your website. This is the recommended approach (especially if you have a website and mobile app). Alternatively you can use [custom URL schemes](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content/defining_a_custom_url_scheme_for_your_app), which can be quicker to get going with.

Android [App links](https://developer.android.com/training/app-links/index.html) offer the same functionality on Android, allowing a common URI for your web page and deep links into your native application.

[Back to top](#on-this-page-1)

#### 4\. Exchanging the authorisation code for an access token

You can now call our token exchange endpoint to exchange the authorisation code for an access token. Each access token is valid for a single registered application for a single account holder. You can think of the access token as an encoded username and password specific to your application and the account holder, so treat it as a password, and keep it secure.

For production applications, the token exchange endpoint uses certificate-based mutual TLS (mTLS). You need to supply your OBIE or eIDAS certificate as your client certificate to secure the request.

For sandbox applications, you can choose whether or not to use mTLS. If you have a suitable OBIE or eIDAS certificate, you can use our mTLS-secured endpoint. Otherwise, we provide a non-mTLS endpoint that you can call without a certificate.

See [accessing live Starling Bank accounts](#accessing-live-starling-bank-accounts-1) for more details about the certificates we accept and how to register them.

The token exchange endpoints are:

- Sandbox (non-mTLS): `https://api-sandbox.starlingbank.com/oauth/access-token`
- Sandbox (mTLS): `https://token-api-sandbox.starlingbank.com/oauth/access-token`
- Production (mTLS): `https://token-api.starlingbank.com/oauth/access-token`

The request and response parameters are the same for all token exchange endpoints.

**NOTE**

- The client secret must not be shared publicly, so you **must** make the call to our token exchange endpoint from your server. You **must not** send the client secret from a browser.
- Also, you **must** send the request parameters in an `application/x-www-form-urlencoded` request body. You **must not** use query parameters.

**Method:** `POST`

**Request body:** `application/x-www-form-urlencoded`

**Request parameters:**

| Parameter | Value |
| --- | --- |
| `code` | The authorisation code you received as the `code` query parameter in step 3 |
| `client_id` | The unique client identifier of your registered application |
| `client_secret` | The client secret for your registered application (how we verify that the request is really from you) |
| `grant_type` | For an authorisation code exchange this must be `authorization_code` |
| `redirect_uri` | The redirect URI the account holder was sent to when Starling returned with the access code in step 3 (this is validated as a further security check) |

Unless you are using the sandbox non-mTLS endpoint, you must supply your OBIE or eIDAS certificate as the mTLS client certificate to secure the request.

The response returns the details of the access token in `application/json` format, for example:

```json
{
    "access_token": "eyJhbGciOiJQUzI1NiIsInppcCI6IkdaSVAifQ.H4sIAAQ-_pT1TALVKOt8.uLjFJikw--DDv3Pf",
    "refresh_token": "De2yfE70KcoFkaEQvVW236s2UVaVexzKGUjHYAG2OGutlO5GLJiSN0rv2P8dean1",
    "token_type": "Bearer",
    "expires_in": 3600,
    "scope": "account:read balance:read card-control:edit card:read transaction:read"
}
```

| Property | Description |
| --- | --- |
| `access_token` | The secret access token to authenticate your requests to our API on behalf of the end user |
| `refresh_token` | The secret refresh token to use to obtain a new access token when the existing token is near expiry |
| `token_type` | The type of token that has been issued - this is always `Bearer` |
| `expires_in` | Number of seconds until the access token expires |
| `scope` | A space-separated list of access scopes granted by the account holder |

You should treat `access_token` and `refresh_token` as arbitrary length strings.

[Back to top](#on-this-page-1)

#### 5\. Calling the Starling APIs

That's it! You now have the access token needed to call the [Starling APIs](#api-reference-1) on behalf of the account holder.

Each API endpoint specification includes the access scope required to call that endpoint. If you call an endpoint using an access token that does not include the correct scope, you receive in a `403 Forbidden` error.

When making requests, authenticate with the `Authorization` header set to `Bearer $access_token`. For example:

```markdown
Bearer eyJhbGciOiJQUzI1NiIsInppcCI6IkdaSVAifQ.H4sIAAQ-_pT1TALVKOt8.uLjFJikw--DDv3Pf
```

We recommend that you include a `User-Agent` header that allows us to identify you if we need to contact you.

[Back to top](#on-this-page-1)

#### 6\. Refreshing your tokens

Later, you need to refresh your access token and refresh token.

To minimise the period of risk if an access token is compromised, the access token expires after a predetermined period (1 hour in production, 24 hours in Sandbox). To carry on accessing resources on the account holder's behalf, you must refresh the access token using the refresh token retrieved in **Step 4**. Refresh tokens are long-lived (6 months) and you can use them even if the associated access token has expired.

When you refresh the access token, you also receive a new refresh token. You should use the new refresh token the next time you refresh the access token. The last-used token will remain available as a back-up for a short time, until you successfully call the token refresh endpoint with a newer refresh token.

When we issue a new refresh token we:

- mark the new refresh token we issued as the new current refresh token
- retain the last-used refresh token as a short term back-up until the newly-issued refresh token is used
- invalidate any other previously-issued refresh tokens, if applicable

You may need to use the back-up refresh token if you don't receive the latest refresh token (for example, if there is a network error) or if you make a duplicate refresh call in another thread and inadvertently invalidate the refresh token you've just received.

If the access token expires, you cannot access the account holder's resources, but, unless the refresh token has also expired, you can still refresh the access token to regain access. If the account holder revokes the access token, or the access token is compromised, the account holder must re-authorise the application.

To refresh your tokens, call the appropriate token exchange endpoint:

- Sandbox (non-mTLS): `https://api-sandbox.starlingbank.com/oauth/access-token`
- Sandbox (mTLS): `https://token-api-sandbox.starlingbank.com/oauth/access-token`
- Production (mTLS): `https://token-api.starlingbank.com/oauth/access-token`

The request and response parameters are the same for all token exchange endpoints.

**NOTE**

- The client secret must not be shared publicly, so you **must** make the call to our token exchange endpoint from your server. You **must not** send the client secret from a browser.
- Also, you **must** send the request parameters in an `application/x-www-form-urlencoded` request body. You **must not** use query parameters.

**Method:** `POST`

**Request body:** `application/x-www-form-urlencoded`

**Request parameters:**

| Parameter | Value |
| --- | --- |
| `refresh_token` | The refresh token received with the access token |
| `client_id` | The unique client identifier of your registered application |
| `client_secret` | The client secret for your registered application (how we verify that the request is really from you) |
| `grant_type` | For a refresh token exchange this must be `refresh_token` |

Unless you are using the sandbox non-mTLS endpoint, you must supply your OBIE or eIDAS certificate as the mTLS client certificate to secure the request.

The response is in the same format as the response to the initial token exchange in step 4.

[Back to top](#on-this-page-1)

As well as the standard errors defined by the [OAuth2](https://tools.ietf.org/html/rfc6749) specification, you may receive one of these client side error codes if the OAuth authorisation process returns an HTTP Status `400`:

| HTTP Status | Error | Error description |
| --- | --- | --- |
| `400` | `invalid_request` | `grant_type must be provided` |
| `400` | `invalid_request` | `grant_type must be either authorization_code or refresh_token` |
| `400` | `invalid_request` | `Unexpected parameter: 'parameterName'` |
| `400` | `invalid_request` | `Incorrect content type. Must be application/x-www-form-urlencoded` |

[Back to top](#on-this-page-1)

### V2 webhooks

#### About V2 webhooks

Our V2 webhooks allow you to register for feed item events, such as card transactions, cash withdrawals and direct debit payments, and standing order events, such as creation, deletion and payments.

You can create personal access token webhooks to receive notifications about your own account, or application webhooks (both sandbox and production) to receive notifications about your application users' accounts.

The V2 webhook payload includes more information about the event than our original (V1) webhook payloads, so you don't need an additional API call to retrieve the event data.

The V2 webhooks use a public-private key pair for additional security, whereas our V1 webhooks signed each request with a shared secret. When you create a V2 webhook, we create a key pair and give you the public key. When we notify you about an event, we use the corresponding private key to sign the request. You use the public key to verify the signature, so you can confirm it has really come from us.

The V2 webhook specifications are at the end of our [API reference documentation](#api-reference-1).

#### Creating V2 webhooks

You create V2 webhooks in the Developer Portal.

To create a webhook for a personal access token, go to the [Personal Access page](https://developer.starlingbank.com/personal/list) and scroll down the page to the **Personal Access V2 Webhooks** section. Click **Create Webhook**.

To create a webhook for an application, choose your application from the [application list](https://developer.starlingbank.com/application/list), select the **V2 Webhook** tab and choose whether you want to create a production or sandbox webhook. Then click **Create webhook**.

In both cases, specify the webhook name and payload URL, select the events you want to be notified about, then click **Create Webhook**. If some events are greyed out and cannot be selected, your application doesn't have the access scopes required to access that event data.

When the webhook is created, we give you the public key part of the key pair we've generated. Later, we'll use the private key to sign the webhook notifications we send to you.

Note that, due to firewall rules, the payload URL you specify cannot be the default hostname of an AWS API gateway endpoint hosted in the `eu-west-1` region, such as `<restapi-id>.execute-api.eu-west-1.amazonaws.com`. If this scenario applies to you, there are several alternatives. For example, you could deploy the API gateway in another AWS region, or set up a custom domain name for the API gateway endpoint.

#### V2 webhook permissions

Each webhook is protected by an Oauth scope. You'll only be able to create webhooks that you have the OAuth scope to access. The V2 webhook specifications in the [API reference section below](#api-reference-1) include the scopes you need.

#### V2 webhook security

It is important that you validate that notifications have originated from Starling's servers and have not been tampered with in transit.

We use **SHA512withRSA** to calculate the payload digest and sign it with the private key part of your webhook's key pair. We place the signature in the request header in `X-Hook-Signature`.

You must use your webhook's public key to decrypt the signature and obtain the payload digest, then compare it with the payload digest you calculate directly from the raw payload data. If the two match, you can be confident the message has originated from us and has not been tampered with in transit.

Make sure the library you are using provides access to the raw JSON payload. If the payload is modified or formatted in any way, you will be unable to validate the signature.

#### V2 webhook retries

If your webhook endpoint does not return a 2XX response within two seconds, we retry three more times with back-off:

- if the first attempted delivery fails, we schedule another delivery after another two minutes
- if the second attempted delivery fails, we schedule another delivery after another four minutes
- if the third attempted delivery fails, we schedule another delivery after another eight minutes
- if the fourth attempted delivery fails, we give up

#### V2 webhook events

We send V2 webhook events to a sub-resource of the base payload URL you specified when you created the webhook.

- When a standing order is created or modified, we send a **standing order** webhook event to `<base-payload-url>/standing-order`
- When a standing order payment is attempted, we send a **standing order payment** webhook event to `<base-payload-url>/standing-order-payment`
- When a feed item transaction or update occurs, we send a **feed item** webhook event to `<base-payload-url>/feed-item`

The payload contains the event details.

If you have registered for both standing order payment webhook events and feed item webhook events, you receive both webhook notifications when a standing order payment is attempted.

#### V2 webhook examples

You can find V2 webhook code examples on [GitHub](https://github.com/starlingbank/api-samples/tree/master/public-api-examples/webhook-verification/v2-webhooks).

[Back to top](#on-this-page-1)

### API keys and message signing

Some API requests require stricter levels of security (for example, for payment instruction). We use digital signatures to validate integrity and provide non-repudiation.

The overview of the process is:

1. You generate two key pairs:
	- an API key pair (public and private keys) to sign API requests
		- a rotation key pair (public and private keys) to sign new API key uploads
2. You upload the public key from each key pair to the Developer Portal
3. You make signed requests (using the private API key) to endpoints that require signatures
4. Later, you can rotate the API keys, using your private rotation key to sign the new public API key when you upload it.

[Back to top](#on-this-page-1)

#### 1\. Generate key pairs

You'll need two public/private key pairs - an API key to sign API requests, and a rotation key to sign new API key uploads.

In the production environment, you must keep the private keys of both key pairs secure.

Key requirements:

- Keys must be either RSA or ECDSA keys.
- RSA keys should have a length of either 2048 or 4096.
- ECDSA keys should have a length of 256.
- RSA keys should not be SSH keys. All valid RSA key bodies start `MII`.

You can generate RSA key pairs satisfying these requirements using [OpenSSL](https://www.openssl.org/):

```bash
openssl genpkey -out starling-api-private.key -algorithm RSA -pkeyopt rsa_keygen_bits:4096
openssl rsa -in starling-api-private.key -pubout -out starling-api-public.key

openssl genpkey -out starling-rotation-private.key -algorithm RSA -pkeyopt rsa_keygen_bits:4096
openssl rsa -in starling-rotation-private.key -pubout -out starling-rotation-public.key
```

[Back to top](#on-this-page-1)

#### 2\. Upload public keys

To upload keys, in the Developer Portal, go to the **Keys** section of your [registered application](https://developer.starlingbank.com/application).

Here you can select the environment (production or sandbox) and add your public API key and public rotation key.

The key should be one of the following:

- a complete public key, across new lines, and with headers and footers such as `-----BEGIN PUBLIC KEY-----`
- the whole body of the public key in a single line (beginning `MII`) without headers and footers

**Tip**: to quickly copy keys to the clipboard you can use:

```bash
# Windows
cat starling-api-public.key | clip
cat starling-rotation-public.key | clip

# Mac
cat starling-api-public.key | pbcopy
cat starling-rotation-public.key | pbcopy

# Linux
cat starling-api-public.key | xclip
cat starling-rotation-public.key | xclip
```

[Back to top](#on-this-page-1)

#### 3\. Make signed requests

Now you're ready to use your private API key to make signed requests to the API.

The content to be signed is a list of key-value pairs including the **request URL**, the **date**, and the **request body**. The content to be signed can optionally include other HTTP headers from the request, but this is not mandatory.

You sign this content with your API key, then include the signature in a `Signature` field in the `Authorization` header of your API request.

**The content to be signed**

The content to be signed is:

```markdown
(request-target): <request-target>
Date: <date>
Digest: <digest>
<optional-additional-header>: <additional-header-value>
```

where:

- `<request-target>` is the path of the requested resource, excluding the host name, for example `put /api/v2/payments/local/account/bbbbbbbb-bbbb-4bbb-bbbb-bbbbbbbbbbbb/category/cccccccc-cccc-4ccc-cccc-cccccccccccc`
- `<date>` is the ISO 8601 date and time, with offset, when the request was made, for example, `2020-04-07T18:26:30.668978+01:00`. Note that:
	- you must make the request within 5 seconds of this time
		- this is not the standard HTTP format for a `Date` header
- `<digest>` is the SHA-512 digest of the request payload (the raw JSON string which makes up the request body), or `X` if no message body is present
- (optional) `<optional-additional-header>` and `<additional-header-value>`, one or more additional HTTP headers and their values to be included in the signature. Any additional headers you include in the content to be signed must also be present in the HTTP request

An example string representation of the content to be signed is:

```markdown
(request-target): put /api/v2/payments/local/account/bbbbbbbb-bbbb-4bbb-bbbb-bbbbbbbbbbbb/category/cccccccc-cccc-4ccc-cccc-cccccccccccc
Date: 2020-04-07T18:26:30.668978+01:00
Digest: Aw4ttuzY7Rn0ad4QFkauxuWrLbgBY3QhtYm8SCKDFY2FlmzKgsk5zwW4CJzdc/8hECqylyXaYyOZsH1/rq1qHQ==
```

**The signature**

The `Signature` field in the `Authorization` header is:

```markdown
Signature keyid="<keyid>",algorithm="<algorithm>",headers="<headers>",signature="<signature>"
```

where:

- `<keyid>` is the UID of the API key used to sign the request. You can find this in the Developer Portal in the **Keys** section of your [registered application](https://developer.starlingbank.com/application)
- `<algorithm>` is the signing algorithm you used to calculate the signature. We support `rsa-sha256`, `rsa-sha512`, `ecdsa-sha256` or `ecdsa-sha512`.
- `<headers>` is a space-delimited list of the elements you have included in the content that is being signed. This must contain `(request-target) Date Digest` and also any other HTTP headers that you optionally included in the content to be signed.
- `<signature>` is the signature generated with your private API key by applying `<algorithm>` to the content to be signed, as above, then encoding the result in Base64.

An example of a complete `Authorization` header, including the `Bearer` field (containing the access token) and the `Signature` field, is:

```markdown
Bearer eyJhbGciOiJQUzI1NiIsInppcCI6IkdaSVAifQ.H4sIAAQ-_pT1TALVKOt8.uLjFJikw--DDv3Pf;Signature keyid="aaaaaaaa-aaaa-4aaa-aaaa-aaaaaaaaaaaa",algorithm="rsa-sha512",headers="(request-target) Date Digest",signature="Wdg4ZakVT1/wLU0o9XlKkxvf8MaYQWxUIxWq2Z+/1NvD6NXvqkCOFgM/7Nblh8rFKDenmf0iWtS5DSJmBh5Sjg/ZT4OV44Wm0f0zQ2BIVlENHduMj8UqWRLBgq11HWOILF1UIKBhd92iv0huQIohPskbiHkXgW2NUr6qsVUmljY4zClh0hW48WI5sRRDHO9TqTYDBec0mAOiHnSNGVKGps4KORanbCGmqwythxgxMq3es4IgkeBxe5AEtdI13HT73WqfXIi0lZkgTS5vKyW8ieitNvhBq1rCBLrnnTp3BoTPNevVCbOOjvOknWZD4It1XXHdbz0oCAUl0FVLEOfjB8oXO+ss91TuXiIgJ8zq7fvNJUEoYNYxW8NZ41+MqCizZiK3cbTWrDMMvRtB/muCm6CsUp8o8C/wKRo90+jACGn8xVw/WUx1+fWGCqmFFFhIikqFnkamDBjzc+aHseHuQ09GkrGG1SK4yM4k5ffVZFO1HnInRAS7q/n8tW7YFvFjXXX1lT2OvzAAaGGtnSluezKoHYoN1a+URO2vy0tmiFGxMMoXtdKcZX5rCljO3oTe/llYE4YOataa+zj6Y5Xd4bo04TnD+V8bXqf7VUnOUjlaTQDwIft2uQhHKCe3A/WSXoqc0BukRN46Ms+08KFCLVCYYJWYhgcipg38T5GkAg0="
```

**The complete request**

As well as the `Authorization` header, the completed request must include at least the `Date` and `Digest` headers, and any other headers you optionally included in the content to be signed.

An example cURL request that pulls all this together is:

```markdown
curl \
 -X PUT \
 -H 'Accept: application/json' \
 -H 'Date: 2020-04-07T18:26:30.668978+01:00' \
 -H 'Digest: Aw4ttuzY7Rn0ad4QFkauxuWrLbgBY3QhtYm8SCKDFY2FlmzKgsk5zwW4CJzdc/8hECqylyXaYyOZsH1/rq1qHQ==' \
 -H 'Authorization: Bearer eyJhbGciOiJQUzI1NiIsInppcCI6IkdaSVAifQ.H4sIAAQ-_pT1TALVKOt8.uLjFJikw--DDv3Pf;Signature keyid="aaaaaaaa-aaaa-4aaa-aaaa-aaaaaaaaaaaa",algorithm="rsa-sha512",headers="(request-target) Date Digest",signature="eWdg4ZakVT1/wLU0o9XlKkxvf8MaYQWxUIxWq2Z+/1NvD6NXvqkCOFgM/7Nblh8rFKDenmf0iWtS5DSJmBh5Sjg/ZT4OV44Wm0f0zQ2BIVlENHduMj8UqWRLBgq11HWOILF1UIKBhd92iv0huQIohPskbiHkXgW2NUr6qsVUmljY4zClh0hW48WI5sRRDHO9TqTYDBec0mAOiHnSNGVKGps4KORanbCGmqwythxgxMq3es4IgkeBxe5AEtdI13HT73WqfXIi0lZkgTS5vKyW8ieitNvhBq1rCBLrnnTp3BoTPNevVCbOOjvOknWZD4It1XXHdbz0oCAUl0FVLEOfjB8oXO+ss91TuXiIgJ8zq7fvNJUEoYNYxW8NZ41+MqCizZiK3cbTWrDMMvRtB/muCm6CsUp8o8C/wKRo90+jACGn8xVw/WUx1+fWGCqmFFFhIikqFnkamDBjzc+aHseHuQ09GkrGG1SK4yM4k5ffVZFO1HnInRAS7q/n8tW7YFvFjXXX1lT2OvzAAaGGtnSluezKoHYoN1a+URO2vy0tmiFGxMMoXtdKcZX5rCljO3oTe/llYE4YOataa+zj6Y5Xd4bo04TnD+V8bXqf7VUnOUjlaTQDwIft2uQhHKCe3A/WSXoqc0BukRN46Ms+08KFCLVCYYJWYhgcipg38T5GkAg0="' \
 -d '{<request-body>}' \
 https://api.starlingbank.com/api/v2/payments/local/account/bbbbbbbb-bbbb-4bbb-bbbb-bbbbbbbbbbbb/category/cccccccc-cccc-4ccc-cccc-cccccccccccc
```

where `<request-body>` is the JSON data being passed to the request.

You can find message signing code examples on [GitHub](https://github.com/starlingbank/api-samples/tree/master/public-api-examples/message-signing).

[Back to top](#on-this-page-1)

#### 4\. (later) Rotate keys

There is no maximum age for keys, but we recommend rotating keys on significant staff departure, or if you think your private API key may be compromised.

To add a second API key, you need to sign your new public API key with your private rotation key. You can then use either the old or new API key in API calls to support seamless rotation. Once you've migrated all your applications over to the new API key, you can expire the old API key to prevent it being used.

To add a new API key:

1. Generate a new API key pair (see step 1, 'Generate key pairs' above).
2. Upload your new public API key to the Developer Portal (see step 2, 'Upload public keys' above).
3. Use your existing private rotation key to sign your new public API key.
4. In the Developer Portal, upload the key signature and the signature timestamp.

**The content to be signed**

The content to be signed is derived from the date and the new public API key as follows:

```markdown
Date: <date>
Digest: <digest>
```

where:

- `<date>` is the ISO 8601 date and time, with offset, when the request was made (for example `2020-04-01T12:34:56+01:00`). You must upload the signature to the Developer Portal within 5 minutes of this time.
- `<digest>` is the SHA-512 digest of the Base64 string of your new public API key. If you have a key in PKCS8 format, the Base64 string is just the public key file without headers and whitespace.

An example string representation of the content to be signed is:

```markdown
Date: 2020-04-07T19:47:56.801792+01:00
Digest: D9fiexHEKiuqWIgg3SEl1S5Dohbg96z6L8VJiT4lHjJxoKZ3OYn023u5hUOQgXiP8mbCPwIWMYZqPBiPtPd9iQ==
```

**The signature**

The format of the signature for a new public API key is:

```markdown
Signature keyid="<keyid>",algorithm="<algorithm>",headers="Date Digest",signature="<signature>"
```

where:

- `<keyid>` is the UID of the rotation key used to sign the request. You can find this in the Developer Portal in the **Keys** section of your [registered application](https://developer.starlingbank.com/application)
- `<algorithm>` is the signing algorithm you used to calculate the signature. We support `rsa-sha256`, `rsa-sha512`, `ecdsa-sha256` and `ecdsa-sha512`.
- `headers` is a space-delimited list of the elements which have been used to create the signature. For key rotation, this must be exactly `Date Digest`.
- `<signature>` is the signature generated with your private rotation key by applying `algorithm` to the content to be signed, as above, then encoding the result in Base64.

An example of a complete signature for a new public API key is:

```markdown
Signature keyid="aaaaaaaa-aaaa-4aaa-aaaa-aaaaaaaaaaaa",algorithm="rsa-sha512",headers="Date Digest",signature="GbJROi2cB9E0A/+WvXBEzfVGJcx7ep868n2lgnkY4GQXBF5e/6g4viilosYwfBY2JrDfhYVcFv/Pd+bBXBebxs/Z75BgvBmen8M8zeV14XbrKPXtKVuz8dnSplEcJ0z1Djucb4c2tb386hkGDeE1eZXKpLr8DEHO1kbDb6Ox65YkMXor0sY7/BrdziumjQMantd4fBwwbHHWJFcTb39c4MCCHJZwhJ8+/Ay9Q3adY3U5c4dTc8cZotKOkKrBbGltY4Cwq0arahz0kkwzERBJbqFOm7O0z638ZPKWK8y2Xgk3wLrFbtuhjf6y70elhTMN/+VtdElWWVTPLqV0r8V1du2gMa+cg4/hwVB6QyUl/QuXB5mQ/TYGzlMI/9S/qclXdnVOLmOQNYLIwah2NCEDL8fl+tWSyiM2dhxSY1/wa7iG89fp60LeOF9qfnKkF16fPgsGaWWhOlxiwQDNnWoEI7Gf4AMWf7iTtqpfx2N5vGwRJtZ7X4yJRt8P1J2sGB9Kau9+Sxq+FcRpZjemyrh0MTw0fpEMcg9zoVkDSTQk+aim4qOqn7hEPErhxAj8x241DYARU/cv4O/tBLu3Dij/FMoj2MJrecVeykqqxYcBfixiKO5O4HZFrbSc8wKAAhAOBjCMo9GG3NroghcCqvcvNeMlAEaKAU7QiUoJcp/yVh8="
```

**The signature timestamp**

The signature timestamp for a new API key is the value of `Date` used to generate the signature. Note that you must upload the new key data within five minutes of this time.

You can find key rotation code examples on [GitHub](https://github.com/starlingbank/api-samples/tree/master/common-examples/key-rotation).

## Starling Bank API1.0.0OAS3

Servers

[[Personal Finance System]]