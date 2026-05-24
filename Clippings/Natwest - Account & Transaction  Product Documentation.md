---
title: "Account & Transaction | Product Documentation"
source: "https://www.bankofapis.com/products/natwest-group-open-banking/accounts/documentation/nwb/4.0.0"
author:
published:
created: 2026-05-23
description: "Join our community and help build the future of banking. Our API ecosystem simplifies and facilitates the development of innovative digital solutions for our customers."
tags:
  - "clippings"
---
Overview

Documentation

Show

NatWest

Version

4.0.0 (Latest)

## Introduction

The Account and Transaction API gives access to the following information relating to an account:

- The Account Product
- Transaction history
- Balances
- Direct Debits
- Standing Orders
- Beneficiaries
- Scheduled Payments
- Offers
- Card Holder Names Endpoint (for ClearSpend corporate credit card users)
- Statements (For Credit Cards)

Each item above is accessed via a separate endpoint. Each of these is documented in detail with examples below.

Throughout the documentation we presume you are a developer wanting to build an application "the client application" to consume customer data.

## Prerequisites

### Partner Access

All of our APIs are available for consumption by our business partners. If you are not already a partner and want to become one, please get in touch via the [contact us](https://www.bankofapis.com/contact-us) page to request access.

### Regulatory Access

Regulatory users are automatically granted access to this API. The steps to gain access are:

- You must be registered with the relevant competent authority as an Account Information Service Provider (AISP). In the UK that is the Financial Conduct Authority (FCA).
- You must have valid transport and signing certificates uploaded to the UK Open Banking Implementation Entity (OBIE) Directory. These can be:
	- *OB Transport* and *OB Signing* certificates issued by the OBIE
		- *OBWAC* and *OBSEAL* ETSI format certificates issued by the OBIE
		- *QWAC* and *QSEAL* PSD2 qualified eIDAS certificates issued by a PSD2 Qualified Trust Service Provider (QTSP) only applies to non-UK-registered TPPs
- You must be on-boarded with NatWest Group as a Third Party. This can be done via the [Dynamic Client Registration](https://openbankinguk.github.io/dcr-docs-pub/v3.2/dynamic-client-registration.html) endpoint. The Dynamic Client Registration endpoint will determine which OAuth Token Endpoint Authentication method you should use, based on the transport and signing certificates. It will be:
	- `tls_client_auth` if you have *OB Transport* and *OB Signing* certificates. This is our [FAPI certified implementation](https://openid.net/certification/#FAPI_OPs).
		- `private_key_jwt` if you have *OBWAC* or *OBSEAL* ETSI format certificates (or *QWAC* and *QSEAL* for non-UK-registered TPPs)

## Customer Consent

Gathering customer consent is the first step of the API journey. Each customer must consent to allow you to access their accounts, where a consent is an agreement between you as the application owner and the customer.

Once consent is agreed between you and the customer, your client application initiates the request for access to the customer's accounts, with authentication and request confirmation happening on the bank's web or mobile platform.

The design of the API ensures that a customer's credentials are never shared with you, so they can be reassured that their credentials remain confidential at all times.

### Gaining Access to Customer Data

The process of gaining access to a customer's data is a multi-step process, all of which must be completed.

### Obtaining an Access Token Using private\_key\_jwt

Firstly, you obtain an Access Token using the [OAuth 2.0 Client Authentication](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) private\_key\_jwt method, signing the JWT with the same signing certificate uploaded to the UK Open Banking Implementation Entity (OBIE) Directory.

When requesting a token you must pass one or more of the OAuth Scopes you have been granted. You may register multiple consents (potentially containing different scopes) using a single Access Token. Remember to present your transport cert when making each request. See [Mutual TLS](#mtls) for more information. The implementation will be client specific; please refer to your client documentation.

#### Example

Example request:

```markdown
POST https://secure1t.natwest.com/as/token.oauth2 HTTP/1.1
Accept: application/json
Content-Type: application/x-www-form-urlencoded; charset=ISO-8859-1

grant_type=client_credentials
&scope=accounts
&client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL2p3dC1pZHAuZXhhbXBsZS5jb20iLCJzdWIiOiJtYWlsdG86bWlrZUBleGFtcGxlLmNvbSIsIm5iZiI6MTQ5OTE4MzYwMSwiZXhwIjoxNDk5MTg3MjAxLCJpYXQiOjE0OTkxODM2MDEsImp0aSI6ImlkMTIzNDU2IiwidHlwIjoiaHR0cHM6Ly9leGFtcGxlLmNvbS9yZWdpc3RlciJ9.SAxPMaJK_wYl_W2idTQASjiEZ4UoI7-P2SbmnHKr6LvP8ZJZX6JlnpK_xClJswAni1Tp1UnHJslc08JrexctaeEIBrqwHG18iBcWKjhHK2Tv5m4nbTsSi1MFQOlMUTRFq3_LQiHqV2M8Hf1v9q9YaQqxDa4MK0asDUtE_zYMHz8kKDb-jj-Vh4mVDeM4_FPiffd2C5ckjkrZBNOK001Xktm7xTqX6fk56KTrejeA4x6D_1ygJcGfjZCv6Knki7Jl-6MfwUKb9ZoZ9LiwHf5lLXPuy_QrOyM0pONWKj9K4Mj7I4GPGvzyVqpaZUgjcOaZY_rlu_p9tnSlE781dDLuw
```

In this case the requested scope is `accounts`. This is the scope you require to access account data. Alternative scopes are available. For example `payment` to access payment services. The `client_assertion` JWT claims:

- `iss` and `sub` must contain the *client\_id* that you obtained during client on-boarding. See [Prerequisites](#prerequisites) for details.
- `kid` must match the Key ID of the signing certificate uploaded to the UK Open Banking Implementation Entity (OBIE) Directory.

Non-Base64 client\_assertion JWT

```json
{
  "kid": "B8k7lIkZSVDNO0W_9wE_B5OGm28",
  "typ": "JWS",
  "alg": "PS256"
}
.
{
  "sub": "wXxoy2tIuy4TbiDNsWOTVJ",
  "aud": "https://secure1t.natwest.com/as/token.oauth2",
  "iss": "wXxoy2tIuy4TbiDNsWOTVJ",
  "exp": 1584373475,
  "iat": 1584372875,
  "jti": "8e36d28e-5eda-4bad-b500-58ef64e6b466"
}
.
<<signature>>
```

Response:

```markdown
HTTP/1.1 200 OK
X-Frame-Options: SAMEORIGIN
Referrer-Policy: origin
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache, no-store
Pragma: no-cache
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: PF=DpKX8c9D7Bxl8prKqTOgUQ; Path=/as; Secure; HttpOnly; Domain=secure1t.natwest.com
Content-Encoding: gzip
Content-Type: application/json;charset=utf-8
Content-Length: 102
Date: Mon, 12 Nov 2018 09:54:27 GMT
Server: standalone/v1.0
Set-Cookie: f5avrbbbbbbbbbbbbbbbb=GIBFCB...; HttpOnly; secure

{
    "access_token": "C5JMQLt0yLNPllTkRmLhJ6sTzlmv",
    "token_type": "Bearer",
    "expires_in": 599
}
```

The value of the `access_token` field in the body of this response is your Access Token which you use to register an account access request in the next step.

### Registering an Account Access Request

Once authenticated, you must make an HTTP POST request to the `account-access-consents` endpoint to register your intent to access customer account data. This request contains details of the access you wish to obtain to the customer's account, including:

- Data Clusters (or permissions) requested on the customer's accounts. Examples include the ability to read transactions or read balances;
- First and last transaction dates and times you would like to access (optional);
- Number of days the access will remain valid (defaulted to ongoing if not provided).

At this stage the Account Access Request is in the `AwaitingAuthorisation` state. It cannot be used until it is confirmed by the customer.

The Account Request endpoint is protected by [Mutual TLS](#mutual-tls). The Access Token you received during the previous stage must be passed as the `Authorization` header with every request to this endpoint.

- *With the recent advancements in APIs, TPPs having the permission "NWGReadCreditCardInternalDetail" will be eligible to view Credit Card Missed payments information along with accounts api response. This will include accounts status and past due payments details. TPP needs to request onboarding team to enable access for Credit Card Missed Payments information for them.*

#### Example

Example Account Access Request:

```markdown
POST https://api.natwest.com/open-banking/v4.0/aisp/account-access-consents HTTP/1.1
Authorization: Wdi7eSGbZn58nKGdRsg3MjKzE4c7
x-fapi-customer-last-logged-time: Sun, 16 Sep 2018 11:43:31 UTC
x-fapi-customer-ip-address: 1.2.3.4
x-fapi-interaction-id: 855f6b6f-5f84-4d02-8946-8a3fd0ed6601
Content-Type: application/json
Accept: application/json

{
  "Data": {
    "Permissions": [
      "ReadAccountsDetail",
      "ReadBalances",
      "ReadBeneficiariesDetail",
      "ReadDirectDebits",
      "ReadProducts",
      "ReadStandingOrdersDetail",
      "ReadTransactionsCredits",
      "ReadTransactionsDebits",
      "ReadTransactionsDetail",
      "ReadScheduledPaymentsDetail",
      "ReadStatementsBasic",
      "ReadStatementsDetail"
    ],
    "ExpirationDateTime": "2018-09-23T17:20:11.198Z",
    "TransactionFromDateTime": "2012-01-11T00:00:00Z",
    "TransactionToDateTime": "2018-01-20T00:00:00Z"
  },
  "Risk": {}
}
```

Example Response *Note:-* `ConsentId` below is synonymous with Account Access Request Id:

```markdown
HTTP/1.1 201 Created
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
X-Frame-Options: DENY
x-fapi-interaction-id: 855f6b6f-5f84-4d02-8946-8a3fd0ed6601
Set-Cookie: f5avrbbbbbbbbbbbbbbbb=CKHOL...; HttpOnly; secure; Domain=api.natwest.com; Path=/open-banking/v4.0
Content-Encoding: gzip
Content-Type: application/json;charset=utf-8
Content-Length: 303
Date: Fri, 16 Nov 2018 08:57:56 GMT
Server:

{
    "Data": {
        "ConsentId": "c2158b0021ed42bbb64ee1cc2e534741",
        "Permissions": [
              "ReadAccountsDetail",
              "ReadBalances",
              "ReadBeneficiariesDetail",
              "ReadDirectDebits",
              "ReadProducts",
              "ReadStandingOrdersDetail",
              "ReadTransactionsCredits",
              "ReadTransactionsDebits",
              "ReadTransactionsDetail",
              "ReadScheduledPaymentsDetail",
              "ReadStatementsBasic",
              "ReadStatementsDetail"
        ],
        "CreationDateTime": "2018-09-16T17:20:11.198Z",
        "ExpirationDateTime": "2018-09-23T17:20:11.198Z",
        "TransactionFromDateTime": "2012-01-11T00:00:00Z",
        "TransactionToDateTime": "2018-01-20T00:00:00Z",
        "Status": "AWAU"
    },
    "Risk": {

    },
    "Links": {
        "Self": "https://api.natwest.com/open-banking/v4.0/aisp/account-access-consents/c2158b0021ed42bbb64ee1cc2e534741"
    },
    "Meta": {

    }
}
```

### Confirming the Account Access Request

The next step is to obtain confirmation from the customer for your access request. To achieve this you redirect the customer to our authentication portal where they log in using their banking credentials, view your access request and confirm or decline it. The access is now in the `Authorised` state.

Next, an Authorisation Code is generated by us, and the customer is redirected back to you. The Authorisation Code is provided to you as part of that redirect.

The process is detailed below, step by step.

#### The Authorisation Request

Firstly, you make a call to our Authorisation endpoint. The address of this endpoint is available in the Open Banking Directory. This endpoint is HTTPS, protected with [TLS](#tls-requirements) and HSTS. During this stage you redirect the customer's browser to our Authorisation endpoint to initiate the process of obtaining the customer's confirmation.

A detailed summary of our authorisation servers (including use of Universal Links / Application links) can be found [here](https://www.bankofapis.com/articles/consent-confirmation-support/natwest-group-authorisation-servers-explained).

When making the request your client application constructs and passes us a redirect URL. The Authorisation Request URL you construct and invoke at this stage has a specific form. It must contain:

- Your `client_id`
- A `response_type` of 'code id\_token'.
- A `code_challenge_method` and `code_challenge` query parameter (see [PKCE RFC](https://tools.ietf.org/html/rfc7636))
- An OpenID Connect object in the `request` query parameter in the form of a signed JWT.

The signed JWT must contain a set of mandatory claims including the Intent ID obtained as part of the Account Request API invocation and a `redirect_uri` parameter.

The location of the Redirect URI is used to return the customer to you once access has been granted using our authentication portal. The root of the redirect URI must match that registered by you at registration time. See [Prerequisites](#prerequisites) for details on registration.

You must sign the JWT containing all the claims using the signing certificate ("sig" key) issued by the relevant Certificate Authority, for example the OBIE in the UK. See the example request below and the [Open ID Connect Specification](https://openid.net/specs/openid-connect-core-1_0.html) for details.

*Note:-* At this time the NatWest Authorisation endpoint only supports use of RSA256 for JWT signing.

#### Example

Example Authorisation Request. This example uses `https://developer.natwest.com/dummy_redirect.htm` as an example redirect URL:

```markdown
GET https://secure1t.natwest.com/as/authorization.oauth2?client_id=7xPKBspndegEsfR2f2Fss2s&response_type=code%20id_token&code_challenge_method=S256&code_challenge=GGQfwpOUSD3-TbAC0jbrUR-CdLKuY5grWwGjTP4Hzwk&request=eyJraWQiOiJQWnJ3RUl3VW8ydmhyajBEZU0xOWNLUVNOczQiLCJ0eXAiOiJKV1MiLCJhbGciOiJSUzI1NiJ9.eyJjb25zZW50UmVmSWQiOiJjMjE1OGIwMDIxZWQ0MmJiYjY0ZWUxY2MyZTUzNDc0MSIsInNjb3BlIjoib3BlbmlkIGFjY291bnRzIiwiYWNyX3ZhbHVlcyI6InVybjpvcGVuYmFua2luZzpwc2QyOmNhIiwiaXNzIjoiNnhQVUh4bVNtTjhsZkwyRmJ6dXgzcyIsImNsYWltcyI6eyJpZF90b2tlbiI6eyJhY3IiOnsiZXNzZW50aWFsIjp0cnVlfSwib3BlbmJhbmtpbmdfaW50ZW50X2lkIjp7InZhbHVlIjoiYzIxNThiMDAyMWVkNDJiYmI2NGVlMWNjMmU1MzQ3NDEiLCJlc3NlbnRpYWwiOnRydWV9fSwidXNlcmluZm8iOnsiYWNyIjp7ImVzc2VudGlhbCI6dHJ1ZX0sIm9wZW5iYW5raW5nX2ludGVudF9pZCI6eyJ2YWx1ZSI6ImMyMTU4YjAwMjFlZDQyYmJiNjRlZTFjYzJlNTM0NzQxIiwiZXNzZW50aWFsIjp0cnVlfX19LCJyZXNwb25zZV90eXBlIjoiY29kZSBpZF90b2tlbiIsInJlZGlyZWN0X3VyaSI6Imh0dHBzOlwvXC9kZXZlbG9wZXIubmF0d2VzdC5jb21cL2R1bW15X3JlZGlyZWN0Lmh0bSIsInN0YXRlIjoiYWJjZDEyMzQiLCJleHAiOjE1NDIwMTY3NjksIm5vbmNlIjoiOWVkY2MxN2ItOGQxMy00YjgzLTgzZWEtM2RlYjEwNDhlMzQ0IiwiY2xpZW50X2lkIjoiNnhQVUh4bVNtTjhsZkwyRmJ6dXgzcyJ9.gMB_qpu4_2mE-mNTiEyBIlwSuDjIZ60rWLb6kSWavbEOR9p9TBatddVb_M6B1pr6Tz7pr0mgMWn4_i4T5TGewwCNHKbD-pgVLLX4-_R9XD8UBk-a6fUOHEoksWdePN-1WEJ0tapUnvZwPwx_uzjl6deIrYHC5mb6FlRHLGp4XIBKv-plM2-SHf7TR5WXzM0r-XBBUjH5dFWCx5R7TGVnOc7x4kaqu7Cah3js7qv1yAJMoue-BP3oeUeReBlzO-0ziUwbXiAeHeHRpXWlYCqJ5RvBsLQyrOp1P49rUZRdx_mnqnhZWBGAiABALzJ3HDNXEIXm6tSqhmBtZfcsTl-_Cw&nonce=9edcc17b-8d13-4b83-83ea-3deb1048e344 HTTP/1.1
Accept: text/html
Host: secure1t.natwest.com
Body: <none>
```

The request claims are base64url encoded within the `request` query parameter. (They can be decoded using [JWT.io](https://jwt.io/)). *Note:-* `consentRefId` and `openbanking_intent_id` below are synonymous with Account Access Request Id:

```json
{
  "kid": "12345",
  "typ": "JWS",
  "alg": "PS256"
}
{
  "consentRefId": "c2158b0021ed42bbb64ee1cc2e534741",
  "scope": "openid accounts",
  "acr_values": "urn:openbanking:psd2:ca",
  "iss": "7xPKBspndegEsfR2f2Fss2s",
  "claims": {
    "id_token": {
      "acr": {
        "essential": true
      },
      "openbanking_intent_id": {
        "value": "c2158b0021ed42bbb64ee1cc2e534741",
        "essential": true
      }
    },
    "userinfo": {
      "acr": {
        "essential": true
      },
      "openbanking_intent_id": {
        "value": "c2158b0021ed42bbb64ee1cc2e534741",
        "essential": true
      }
    }
  },
  "response_type": "code id_token",
  "redirect_uri": "https://developer.natwest.com/dummy_redirect.htm",
  "state": "abcd1234",
  "exp": 1542016769,
  "nonce": "9edcc17b-8d13-4b83-83ea-3deb1048e344",
  "client_id": "7xPKBspndegEsfR2f2Fss2s"
}
```

#### Authorisation Endpoint Response

Once the customer has confirmed the access request we redirect the customer's browser to the Redirect URL you provided. We append this URL with a hash fragment containing the Authorisation Code (`code`) and an id\_token in the form of a signed JWT. Next, you will exchange the Authorisation Code for a set of credentials used to access the accounts confirmed by the customer.

*Note:-* At this time the NatWest Authorisation endpoint uses RSA256 for ID Token signing.

To remain FAPI compliant you must verify the signature of the signed JWT using the public key available from our JWKS endpoint. The location of our JWKS endpoint is the value of the `jwks_uri` attribute available from our [.well-known](https://openid.net/specs/openid-connect-discovery-1_0.html) endpoint.

The value of the `sub` and `openbanking_intent_id` claims in this JWT are the related Account Access Request ID. This enables you to correlate the response with the requested Account Access Request. Additionally the `c_hash` and `s_hash` fields contain, respectively, hashes of the passed in `state` parameter and returned `code` parameter. For more details on the `c_hash` see the [Open ID Connect](https://openid.net/specs/openid-connect-core-1_0.html) documentation. For `s_hash` see the [FAPI](https://openid.net/specs/openid-financial-api-part-2-wd-02.html#introduction) documentation.

#### Example

Example Authorisation Response

```markdown
https://developer.natwest.com/dummy_redirect.htm#code=vRZs_yPSuzsap3BIW9w7CmzgEgJ3MFCsLVkAAAAE&id_token=eyJhbGciOiJSUzI1NiIsImtpZCI6Ikg4OVBtWGx0VXVlVmFLLS1uVlNZM2c2SlBxYyJ9.eyJzdWIiOiJjMjE1OGIwMDIxZWQ0MmJiYjY0ZWUxY2MyZTUzNDc0MSIsImF1ZCI6IjZ4UFVIeG1TbU44bGZMMkZienV4M3MiLCJqdGkiOiJkdmZoUXhlV3ZxNzZnc082OXhLRjcwIiwiaXNzIjoiaHR0cHM6Ly9pYW0tYXV0aG4tc2l0LW53Yi5tYW5hZ2VkdGVzdC5jb20iLCJpYXQiOjE1NDIwMTY0NzgsImV4cCI6MTU0MjAxNjc3OCwib3BlbmJhbmtpbmdfaW50ZW50X2lkIjoiYzIxNThiMDAyMWVkNDJiYmI2NGVlMWNjMmU1MzQ3NDEiLCJub25jZSI6IjllZGNjMTdiLThkMTMtNGI4My04M2VhLTNkZWIxMDQ4ZTM0NCIsImF1dGhfdGltZSI6MTU0MjAxNjQ3OCwiY19oYXNoIjoiazB1QlhqbF9pdF9tT2JvRE5wSTR1USIsInNfaGFzaCI6IjZjN25Hcmt5X2Voak00MEl2azNwM3cifQ.AycZlfTkTuHhEW_7GyWU_h12XszrNzs4Z47tPA9xPRcjpPG-ret7oNhi1pMlO2uiC7VcXPd_uwg_6V7KbKEj8CkpZ3IRibAMgF12OhNUaYRL8Kboopic9JpvU4SH0wgR4_SfixusjQYm2a7MsT11FFA7vvzZzKDh0mW8NRkin3tZ4PWoD-4NAuO8OFHtQ5ZTCSNqwASXdQlUyuiNYW1xq26irLhmD5E4u6g02urcKB5BCGsqEByVddYlqNB2TwNyJBz3kW033X__icIWEe9isJHqtXhe-7y2wrEHVLwDVTZzN2Z5dXPVJB82JEsXl_qeVTdpDJNdZB7n1HeIIBB6jA&state=abcd1234
```

The response claims are encoded within the `id_token` query parameter. (They can be decoded using [JWT.io](https://jwt.io/)): *Note:-* `openbanking_intent_id` below is synonymous with Account Access Request Id:

```json
{
  "sub": "c2158b0021ed42bbb64ee1cc2e534741",
  "aud": "7xPKBspndegEsfR2f2Fss2s",
  "jti": "dvfhQxeWvq76gsO69xKF70",
  "iss": "https://secure1t.natwest.com",
  "iat": 1542016478,
  "exp": 1542016778,
  "openbanking_intent_id": "c2158b0021ed42bbb64ee1cc2e534741",
  "nonce": "9edcc17b-8d13-4b83-83ea-3deb1048e344",
  "auth_time": 1542016478,
  "c_hash": "k0uBXjl_it_mOboDNpI4uQ",
  "s_hash": "6c7nGrky_ehjM40Ivk3p3w"
}
```

### Exchanging an Authorisation Code for an Access Token

In this step the token endpoint is [*authenticated with private key JWT*](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authenticate-with-private-key-jwt) to exchange the authorisation code generated during the authorisation process for an Access Token, similar to the [access token created earlier to register intent](#obtaining-an-access-token-using-private-key-jwt). This endpoint is protected by [Mutual TLS](#mutual-tls). On submission of an authorisation code it returns:

- An Access Token;
- A Refresh Token;
- A signed JWT containing, amongst other things, the Account Access Request ID corresponding to the returned Access Token.

*Note:-* At this time the NatWest Authorisation endpoint uses RSA256 for ID Token signing.

You can use the latter to correlate the response with the request. To that end, the `sub` and `openbanking_intent_id` claims contain the related Intent ID. Additionally the `c_hash` and `s_hash` fields contain, respectively, hashes of the passed in `state` and `code` parameters. For more details on the `c_hash` see the [Open ID Connect](https://openid.net/specs/openid-connect-core-1_0.html) documentation. For s\_hash see the [FAPI](https://openid.net/specs/openid-financial-api-part-2-wd-02.html#introduction) documentation.

The Access Token has fixed validity. See [Access Token/Refresh Token Exchange](#refreshing-an-access-token) for details on obtaining a new token once the original expires.

#### Code Verifier and PKCE

The token endpoint requires each request to include a `code_verifier` as per the [PKCE RFC](https://tools.ietf.org/html/rfc7636) to prevent man-in-the-middle attacks.

#### Example

Example Request:

```markdown
POST https://secure1t.natwest.com/as/token.oauth2 HTTP/1.1
Accept: */*
Content-Type: application/x-www-form-urlencoded; charset=ISO-8859-1

grant_type=authorization_code
&code_verifier=RczZn-l7WMCUIiKQLNcWBXHVNfb1_IR25rGwY1nNAt8
&code_challenge_method=S256
&code=vRZs_yPSuzsap3BIW9w7CmzgEgJ3MFCsLVkAAAAE
&redirect_uri=https://developer.natwest.com/dummy_redirect.htm
&client_id=7xPKBspndegEsfR2f2Fss2s
&client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL2p3dC1pZHAuZXhhbXBsZS5jb20iLCJzdWIiOiJtYWlsdG86bWlrZUBleGFtcGxlLmNvbSIsIm5iZiI6MTQ5OTE4MzYwMSwiZXhwIjoxNDk5MTg3MjAxLCJpYXQiOjE0OTkxODM2MDEsImp0aSI6ImlkMTIzNDU2IiwidHlwIjoiaHR0cHM6Ly9leGFtcGxlLmNvbS9yZWdpc3RlciJ9.SAxPMaJK_wYl_W2idTQASjiEZ4UoI7-P2SbmnHKr6LvP8ZJZX6JlnpK_xClJswAni1Tp1UnHJslc08JrexctaeEIBrqwHG18iBcWKjhHK2Tv5m4nbTsSi1MFQOlMUTRFq3_LQiHqV2M8Hf1v9q9YaQqxDa4MK0asDUtE_zYMHz8kKDb-jj-Vh4mVDeM4_FPiffd2C5ckjkrZBNOK001Xktm7xTqX6fk56KTrejeA4x6D_1ygJcGfjZCv6Knki7Jl-6MfwUKb9ZoZ9LiwHf5lLXPuy_QrOyM0pONWKj9K4Mj7I4GPGvzyVqpaZUgjcOaZY_rlu_p9tnSlE781dDLuw
```

Example Response:

```markdown
HTTP/1.1 200 OK
X-Frame-Options: SAMEORIGIN
Referrer-Policy: origin
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache, no-store
Pragma: no-cache
Expires: Thu, 01 Jan 1970 00:00:00 GMT
Set-Cookie: PF=PlYIYZCdLmfm8mO5e9yLNc; Path=/as; Secure; HttpOnly; Domain=secure1t.natwest.com
Content-Encoding: gzip
Content-Type: application/json;charset=utf-8
Content-Length: 868
Date: Mon, 12 Nov 2018 09:54:38 GMT
Server: standalone/v1.0
Set-Cookie: f5avrbbbbbbbbbbbbbbbb=MDIH...; HttpOnly; secure

{
    "access_token": "zlJojElfSalTBc1CKVwi3adSHPpr",
    "refresh_token": "EhRx3DsdmpU4J8ISVBeVYj5v5l2I2oDorfEVgbPcft",
    "scope": "openid accounts",
    "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6Ikg4OVBtWGx0VXVlVmFLLS1uVlNZM2c2SlBxYyJ9.eyJzdWIiOiJjMjE1OGIwMDIxZWQ0MmJiYjY0ZWUxY2MyZTUzNDc0MSIsImF1ZCI6IjZ4UFVIeG1TbU44bGZMMkZienV4M3MiLCJqdGkiOiJkdmZoUXhlV3ZxNzZnc082OXhLRjcwIiwiaXNzIjoiaHR0cHM6Ly9pYW0tYXV0aG4tc2l0LW53Yi5tYW5hZ2VkdGVzdC5jb20iLCJpYXQiOjE1NDIwMTY0NzgsImV4cCI6MTU0MjAxNjc3OCwib3BlbmJhbmtpbmdfaW50ZW50X2lkIjoiYzIxNThiMDAyMWVkNDJiYmI2NGVlMWNjMmU1MzQ3NDEiLCJub25jZSI6IjllZGNjMTdiLThkMTMtNGI4My04M2VhLTNkZWIxMDQ4ZTM0NCIsImF1dGhfdGltZSI6MTU0MjAxNjQ3OCwiY19oYXNoIjoiazB1QlhqbF9pdF9tT2JvRE5wSTR1USIsInNfaGFzaCI6IjZjN25Hcmt5X2Voak00MEl2azNwM3cifQ.AycZlfTkTuHhEW_7GyWU_h12XszrNzs4Z47tPA9xPRcjpPG-ret7oNhi1pMlO2uiC7VcXPd_uwg_6V7KbKEj8CkpZ3IRibAMgF12OhNUaYRL8Kboopic9JpvU4SH0wgR4_SfixusjQYm2a7MsT11FFA7vvzZzKDh0mW8NRkin3tZ4PWoD-4NAuO8OFHtQ5ZTCSNqwASXdQlUyuiNYW1xq26irLhmD5E4u6g02urcKB5BCGsqEByVddYlqNB2TwNyJBz3kW033X__icIWEe9isJHqtXhe-7y2wrEHVLwDVTZzN2Z5dXPVJB82JEsXl_qeVTdpDJNdZB7n1HeIIBB6jA",
    "token_type": "Bearer",
    "expires_in": 599
}
```

The response claims are base64url encoded within the value of `id_token` in the response body. (They can be decoded using [JWT.io](https://jwt.io/)). *Note:-* `openbanking_intent_id` below is synonymous with Account Access Request Id:

```json
{
  "alg": "PS256",
  "kid": "abcd-1234"
}
{
  "sub": "c2158b0021ed42bbb64ee1cc2e534741",
  "aud": "7xPKBspndegEsfR2f2Fss2s",
  "jti": "dvfhQxeWvq76gsO69xKF70",
  "iss": "https://secure1t.natwest.com",
  "iat": 1542016478,
  "exp": 1542016778,
  "openbanking_intent_id": "c2158b0021ed42bbb64ee1cc2e534741",
  "nonce": "9edcc17b-8d13-4b83-83ea-3deb1048e344",
  "auth_time": 1542016478,
  "c_hash": "k0uBXjl_it_mOboDNpI4uQ",
  "s_hash": "6c7nGrky_ehjM40Ivk3p3w"
}
```

### Accessing Customer Data

Once the Account Access Request has been confirmed and you have the necessary tokens, you may access the customer's account(s) via the APIs. Multiple authorisation records containing different data clusters may be active for any given customer and client application at any one time. The token pertaining to the correct access request must be passed with every API request, even if multiple access requests are active.

The API specifications further down this page contain request/response examples, including tokens, for each endpoint.

#### Access Validity Period

Confirmation to access the customer's account is dependent to expiry date in the request. Any confirmation given by a customer is valid for the period requested by you or ongoing if expiry date in the request is not provided.

## Security Considerations

The APIs are protected by a number of standards-based controls both at the application level and the network level.

### TLS Requirements

All endpoints are secured using Transport Layer Security (TLS1.2). Browser based journeys are HTTPS with HSTS.

#### Ciphers

The Open Banking Directory will only support the generation of certificates using RSA.

The [FAPI Read Write Specification](https://openid.net/specs/openid-financial-api-part-2.html#tls-considerations) specifies the algorithms that should be used for TLS (Section 8.5) and for digital signatures (Section 8.6).

In accordance with Section 8.5, only the following ciphers will be supported for TLS:

- TLS\_DHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256
- TLS\_ECDHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256
- TLS\_DHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384
- TLS\_ECDHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384

#### Mutual TLS

All API endpoints (excluding the browser based consent authentication journey) require you to authenticate yourselves using mTLS by passing your X.509 digital certificate. You must send the entire certificate chain with the certificate otherwise the request will be rejected with a 403 error code.

### API Endpoints

Each API endpoint delivering customer data is protected by the following controls:

- [Mutual Transport Layer Security (TLS1.2)](#mutual-tls)
- Access Token - The OAuth Access Token must be valid and belong to the client application initiating the request.
- Access confirmation - The Access Token and Intent ID associated with the data request must correlate with one another. Furthermore, the data clusters held in the access token must allow access to the requested resource(s).
- Scope - The scope of the Access Token must correspond to the scope required to access that particular API endpoint. In the case of the Account and Transactions API this is the AISP scope.

#### Required Headers

We recommend passing a globally unique `x-fapi-interaction-id` header to be used as a correlation ID, this should be unique for each and every interaction with us. If no `x-fapi-interaction-id` header is passed one will be generated by us. The generated value will be a GUID. In either case the value of this header is returned in the response.

*Note:-* Please include the x-fapi-interaction-id in any support requests. It will greatly expedite the investigation.

#### Refreshing an Access Token

Each Access Token lasts for only a limited period. Once expired, the Refresh Token must be used to generate a new Access Token. This can happen any number of times while the Refresh Token remains valid. Tokens are refreshed using the Token endpoint. This endpoint is protected by [Mutual TLS](#mutual-tls) and uses the private key jwt authentication method [as seen earlier](#obtaining-an-access-token-using-private-key-jwt).

Example Refresh Request:

```markdown
POST https://secure1t.natwest.com/as/token.oauth2 HTTP/1.1
Accept: application/json
Content-Type: application/x-www-form-urlencoded; charset=ISO-8859-1

grant_type=refresh_token
&client_id=7xPKBspndegEsfR2f2Fss2s
&refresh_token=EhRx3DsdmpU4J8ISVBeVYj5v5l2I2oDorfEVgbPcft
&client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJodHRwczovL2p3dC1pZHAuZXhhbXBsZS5jb20iLCJzdWIiOiJtYWlsdG86bWlrZUBleGFtcGxlLmNvbSIsIm5iZiI6MTQ5OTE4MzYwMSwiZXhwIjoxNDk5MTg3MjAxLCJpYXQiOjE0OTkxODM2MDEsImp0aSI6ImlkMTIzNDU2IiwidHlwIjoiaHR0cHM6Ly9leGFtcGxlLmNvbS9yZWdpc3RlciJ9.SAxPMaJK_wYl_W2idTQASjiEZ4UoI7-P2SbmnHKr6LvP8ZJZX6JlnpK_xClJswAni1Tp1UnHJslc08JrexctaeEIBrqwHG18iBcWKjhHK2Tv5m4nbTsSi1MFQOlMUTRFq3_LQiHqV2M8Hf1v9q9YaQqxDa4MK0asDUtE_zYMHz8kKDb-jj-Vh4mVDeM4_FPiffd2C5ckjkrZBNOK001Xktm7xTqX6fk56KTrejeA4x6D_1ygJcGfjZCv6Knki7Jl-6MfwUKb9ZoZ9LiwHf5lLXPuy_QrOyM0pONWKj9K4Mj7I4GPGvzyVqpaZUgjcOaZY_rlu_p9tnSlE781dDLuw
```

Example Response:

```markdown
HTTP/1.1 200 OK
Content-Encoding: gzip
Content-Type: application/json;charset=utf-8
Content-Length: 102

{
  "access_token": "x2gZxLWT3QkzAcXPrRMJVZXDGfJG",
  "refresh_token": "Whrc0SFjBRjqVW0cP5lUic7zC3YknTwPq2ItvTPfDN",
  "token_type": "Bearer",
  "expires_in": 599
}
```

#### Access and Refresh Token Validity Period

Each Access Token lasts 10 minutes. The latest Refresh Token will last until exchanged for a new Access and Refresh Token.

## Balances

Balances returned from the API will have one of two types. See below for an explanation of how each type is computed and what sort of transactions are taken into consideration.

### Expected

Balance of the customer’s account (booked and pending items) including the following:

- Cash withdrawals
- Cash deposits
- Faster Payments
- Pending authorisations
- Point of sale transactions
- Forward posted transactions
- Available overdraft (for accounts outside of the scope of HCC\*)

If an account has an overdraft this will have CreditLine/Included set as False for accounts in scope of HCC\* and True for non-HCC\* accounts in our API response, and the CreditDebitIndicator will be either Credit or Debit based on the balance.

#### Credit card

Balance of the customer’s credit limit taking the money spent on the card into account (credit agreed minus amount spent). This will have CreditLine/Included set as True in our API response, and the CreditDebitIndicator will be Debit.

### ForwardAvailable

Balance of the customer’s account (booked) plus:

- Cash withdrawals
- Cash deposits
- Faster Payments
- Forward posted transactions

If an account has an overdraft this will have CreditLine/Included set as False in our API response, and the CreditDebitIndicator will be either Credit or Debit based on the balance.

#### Credit card

How much has been spent on the card, including pending transactions. There will be no CreditLine information in our API response, and the CreditDebitIndicator will be Debit when there is a balance to be paid back on the card, or is zero, or will be Credit when the balance has been overpaid.

## Beneficiaries

### Beneficiary Type

Each beneficiary has an optional beneficiary type.

If the type is not set then the beneficiary is trusted.

## Transactions Endpoint

The transactions endpoint returns a customer's transaction data, optionally restricted to a particular period of time.

### Transaction Statuses & Mutability

The 'status' field of a transaction can be one of the following: 'BOOK', 'PDNG', 'FUTR' or 'INFO'. Transactions start as 'PDNG' until they have been posted, at which point they become 'BOOK'.

Before a transaction has been posted it is mutable and some fields may change. 'PDNG' transactions are all mutable until they get posted. A 'BOOK' transaction is immutable by default but this can be specified using an optional 'TransactionMutability' flag. This flag can have a value of either 'mutable' or 'immutable'. For more details around transaction mutability see the [Open Banking specification](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Transactions.html#mutability). This flag is not present in the default cases.

To align with the transaction statuses provided by Mobile Banking, the status of intra-day transactions that have not yet posted will be set to 'BOOK'. These transactions can still change, so they will have a TransactionMutability flag set to 'mutable'. Once an intra-day transaction has posted, it will have a status of 'BOOK' and no TransactionMutability flag.

Only transactions that are 'BOOK' and immutable will have a transaction ID. This is a unique ID that won't change. The ID can be used to ensure that a given transaction is the same across different calls to the API. Transactions that have other statuses and transactions that are 'BOOK' but mutable will not have an ID.

The following table outlines the possible combinations:

| Transaction Status | TransactionMutability Flag | Mutability Status | Has Transaction ID |
| --- | --- | --- | --- |
| PDNG | N/A | Mutable | No |
| BOOK | Mutable | Mutable | No |
| BOOK | None | Immutable | Yes |

The following table lists the transaction codes for intra-day payments:

| Code | Description |
| --- | --- |
| BAC | Automated Credit |
| BSP | Branch Single Payment |
| C/L | Cash Withdrawal |
| C/R | Credit Remittance |
| CDM | Cash & Deposit Machine |
| CHG | Account Charges |
| CHP | CHAPS Transfer |
| D/D | Direct Debit |
| DIV | Dividend |
| DPC | Online Transaction |
| EBP | Bill Payment |
| IBP | Inter Branch Payment (Transfer) |
| INT | Interest |
| ITL | CHAPS/International Payment |
| NDC | No Dividend C/foil |
| POC | Post Office Counters |
| S/O | Standing Order |
| SBT | Transfer - Funds Transfer |
| TLR | Teller Transaction |
| TSU | Telephone Banking |

### Transaction Types

There are different types of transactions, and a particular transaction's type is not fixed and may change with time. The following table outlines the relevant transaction types:

| Transaction Type | Description |
| --- | --- |
| Pending | A transaction that is currently being processed |
| Posted | A transaction that has been processed |
| Forward posted | A transaction that will be processed on a particular date |

### Transaction Codes

The following table lists all possible ProprietaryBankTransactionCode/Code values:

| Code | Description |
| --- | --- |
| ADV | Separate Advice |
| AMD | Amendments History |
| BAC | Automated Credit |
| BAE | Branch Acc Entry |
| BCA | Clean Acceptance |
| BCO | Non Mkt Close Out |
| BCR | Clean Reimburse |
| BGC | Credit |
| BGT | Guarantees |
| BOE | Bill of Exchange |
| BSP | Branch Single Payment |
| C/L\* | Cash Withdrawal |
| C/R | Credit Remittance |
| CAE | Cheque Collection |
| CARD\* | Card Payment or Cash |
| CCB | Cheque Collection |
| CCD | Cheque at Despatch |
| CDM | Cash & Deposit Machine |
| CHG | Account Charges |
| CHG TO | Charges |
| CHP | CHAPS Transfer |
| CHQ | Cheque |
| CNA | Clean Cheque Neg |
| CND | Cheque Negotiation |
| D/C | Credit |
| D/D | Direct Debit |
| DCR | Documentary Credit |
| DFT | Foreign Draft |
| DIV | Dividend |
| DPC | Online Transaction |
| EBP | Bill Payment |
| ECA | eurocheque |
| ECD | eurocheque |
| IAT | Transfer |
| IBP | Inter Branch Payment (Transfer) |
| ICP | Inward Ccy Payment |
| IFT | Int Free Threshold |
| INT | Interest |
| INT TO | Interest |
| INTCTO | Interest |
| IPB | Inland Payments |
| IP\*\* | Inland Payments |
| ISP | Inward Stg Payment |
| ITL | CHAPS/International Payment |
| ITM | Incoming CHAPS |
| LON | New Loan |
| LST | Bulk Entry |
| LVP | Low Value Payment |
| MEC | Export Credits |
| MFD | Maturing Fwd Deal |
| MGT | Bonds & Guarantees |
| MIB | Inward Bills |
| MIC | Import Credits |
| MKD | Market Deal |
| MOB | Outward Bills |
| MSC | Standby Credits |
| NDC | No Dividend C/foil |
| NOS | Overseas |
| NVD | Novated Deal |
| ODL | Overdraft Limit |
| ODR | Overdraft Rate |
| OSE | Overseas |
| OTM | Outgoing CHAPS |
| POC | Post Office Counters |
| POS | Card Transaction |
| PYR | Payment/Receipt |
| RFW | Transfer |
| RTF | Relay Transfer |
| RYD | Transfer |
| S/O | Standing Order |
| SBT | Transfer - Funds Transfer |
| SDE | Urgent Euro Tfr |
| STF | Standard Transfer |
| STL | Settlement |
| TCA | T/Chq Negotiation |
| TCD | T/Chq Negotiation |
| TFP | Trade Fin Product |
| TFR | Transfer |
| TLR | Teller Transaction |
| TMS | Overseas - travel money service |
| TSU | Telephone Banking |
| U/D | Unpaid Direct Debit |
| UTF | Urgent Transfer |
| VCS | Visa Cash |

#### Credit card

| Code | Description |
| --- | --- |
| PR | PURCHASE |
| CA | CASH |
| FE | FEE |
| PY | PAYMENT |

\*These transaction codes may be followed by a space and a single digit number. For example 'C/L 1' or 'CARD 5'.

\*\*This transaction code is followed by a single digit number. For example 'IP3'.

### Transaction Filtering

The transaction data returned from this endpoint will be filtered based on the transaction type and whether it is a working day or not. The below table shows the different possibilities. A blank 'toBookingDateTime' field is treated as if it was populated with today's date. The 'fromBookingDateTime' field is ignored for filtering purposes.

| toBookingDateTime | Type of day | Transactions returned |
| --- | --- | --- |
| Today (or blank) | Working day | All transactions |
| Today (or blank) | Non-working day | Pending and posted transactions |
| Before today | Working day | Posted transactions only |
| Before today | Non-working day | Posted transactions only |

Due to this filtering mechanism, fewer than the usual maximum number of transactions may be returned per page. There may still be more transactions available on later pages. Check for the presence of the next page field - if it is present, there are more transactions on that page.

### Transaction date history availability

Requests for full transaction date history will only be fulfilled within the first 180 minutes of customer consent authentication. Outside this authentication period, requests for transaction history will be limited to the last 90 days only.

Where more than 180 minutes since customer consent authentication has elapsed:

- Requests sent where the fromBookingDateTime, in the request or consent, is prior to 90 days ago will only receive transaction data for the last 90 days in the API response.
- Requests sent where the toBookingDateTime, in the request or consent, is prior to 90 days ago will receive a 403 API response.

However, TPPs can request customer re-authentication, enabling them to also request full transaction date history for a further 180 minutes.

## Card Holder Names Endpoint (for ClearSpend corporate credit card users)

This API endpoint provides card holder names for ClearSpend corporate credit card users.

The endpoint returns a list of the card holder's name along with accountid and the card's last 4-digit number, which is associated with the admin account. The information provided includes cardLast4, accountId, name, accountType and expirationDate.

The card holder's information can be retrieved via GET API: open-banking/v4.0/aisp/accounts/{accountId}/card-holder-names.

Example response:

```json
{
    "Data": {
        "AccountId": "E6C604494E9B87B95B497C79BAA4BD88",
        "CompanyId": "0024824",
        "Company": [
            {
                "cardLast4": "9966",
                "accountId": "80000021506",
                "name": "NOS CORPIIX",
                "accountType": "CORPORATE",
                "expirationDate": "2025-06-02"
            },
            {
                "cardLast4": "0014",
                "accountId": "00000034294",
                "name": "NOS CARD 3",
                "accountType": "INDIVIDUAL",
                "expirationDate": "2025-06-02"
            }
        ]
    },
    "Links": {
        "Self": "https://sit-api.natwest.com/open-banking/v4.0/aisp/accounts/E6C604494E9B87B95B497C79BAA4BD88/card-holder-names"
    },
    "Meta": {

    }
}
```

## Statements Endpoint

The statement endpoint returns historical statement information on an account. Information includes statement dates, previous balances, purchase totals, payment information and more.

The statement information can be retrieved via GET /{accountId}/statements endpoint.

The TPPs having the permission "NWGReadCreditCardInternalDetail" will be eligible to view Credit Card payments information such as payments status (i.e No Pmt, Min Pmt, Full Pmt. etc) and Payment Timing (Late, Early, On-Time. etc) along with statements api response.

\*\* Currently this feature is present for only credit card accounts.

Accessing the above endpoint for a savings or a MTA account will result in status code 403.

The following table shows various amount types returned as part of statement response:

| Balance Type | Description | Customer Type |
| --- | --- | --- |
| UK.OBIE.ClosingBalance | Statement Balance | Personal & Business |
| UK.OBIE.MinimumPaymentDue | Minimum payment due balance | Personal |
| UK.OBIE.StartingBalance | Balance brought forward | Personal & Business |
| UK.OBIE.TotalCredits | Payments to account | Personal & Business |
| UK.OBIE.TotalPurchase | Spending plus adjustment | Personal & Business |
| UK.OBIE.CreditLimit | Card credit limit | Personal & Business |
| UK.OBIE.TotalCashAdvances | Cash advances | Personal & Business |
| UK.OBIE.TotalAdjustments | Adjustments | Business |

### Statements Filtering

The statement data returned from this endpoint will be filtered based on the statement dates(`fromStatementDateTime`, `toStatementDateTime` ) provided and whether a day is a working day or not. A blank `toStatementDateTime` field is treated as if it was populated with today's date.

Due to this filtering mechanism, fewer than the usual maximum number of statement may be returned per page. There may still be more statements available on later pages. Check for the presence of the next page field to determine this - if it is present, there are more statements on that page.

There are few exceptions for Phase-1 delivery as mentioned below:

- Credit Card statement is available for last 3 months only.
- This API is currently designed to show Mandatory values for Phase-1, optional fields may or may not have values, few examples are TotalCashAdvances, TotalCredits, TotalCharges.
- Currently API has mismatch for field “Minimum Payment Due” with hard copy of credit card statement. This will be fixed in phase-2 next year.

## Switched Out Current Accounts

The APIs will now indicate if an account on an active customer consent has switched to a different Account Servicing Payment Service Provider (ASPSP) using the Current Account Switching Service (CASS). This can be obtained via the GET /accounts and GET /accounts/{AccountId} endpoints.

Switched accounts will not appear on consent authorisation screen and as a result it will not be possible to authorise new consents to view switched accounts.

Once all consents containing a specific switched out account expire the Third Party Provider (TPP) will no longer be able to retrieve that switched accounts via the APIs.

Accessing other AISP endpoints, for example /balances or /transactions, using a switched account will result in 400 – Bad Request error.

A switched account will only be populated with an AccountId and a SwitchStatus value of “UK.CASS.SwitchCompleted”, no other fields will be included in the response.

Example response:

```json
{
  "Data": {
    "Account": [
      {
        "AccountId": "D2FAW9C5DFCS7BB9F6AAC4FWS53",
        "SwitchStatus": "UK.CASS.SwitchCompleted"
      }
    ]
  },
  "Links": {
    "Self": "https://api.rbs.co.uk/open-banking/v4.0/aisp/accounts"
  },
  "Meta": {
  }
}
```

## Functionality Matrix

The following table shows the functionality supported for each API version:

| Endpoint | Path | Functionality Supported | v1.1 | v2.0 | v3.1 | v4.0 |
| --- | --- | --- | --- | --- | --- | --- |
| Get Accounts | /accounts | **API available** | ✔ | ✔ | ✔ | ✔ |
|  |  | Account Nickname | ❌ | ❌ | ✔ | ✔ |
| Get Account | /accounts/ | **API available** | ✔ | ✔ | ✔ | ✔ |
|  |  | Account Nickname | ❌ | ❌ | ✔ | ✔ |
| Get Account Transactions | /accounts/{AccountId}/transactions | **API available** | ✔ | ✔ | ✔ | ✔ |
|  |  | Pending authorisations | ❌ | ❌ | ✔ | ✔ |
|  |  | Unique immutable transaction IDs | ❌ | ❌ | ✔ | ✔ |
| Get Account Statements | /accounts/{AccountId}/statements | **API available(for Credit Cards)** | ❌ | ❌ | ✔ | ✔ |
|  |  | Pending authorisations | ❌ | ❌ | ✔ | ✔ |
|  |  | Unique immutable statement IDs | ❌ | ❌ | ❌ | ❌ |
| Get Account Beneficiaries | /accounts/{AccountId}/beneficiaries | **API available** | ✔ | ✔ | ✔ | ✔ |
| Get Account Balances | /accounts/{AccountId}/balances | **API available** | ✔ | ✔ | ✔ | ✔ |
|  |  | Overdraft information | ❌ | ❌ | ✔ | ✔ |
| Get Account Direct Debits | /accounts/{AccountId}/direct-debits | **API available** | ✔ | ✔ | ✔ | ✔ |
|  |  | SEPA Direct Debits (only available for Bankline customers) | ❌ | ❌ | ✔ | ✔ |
| Get Account Standing Orders | /accounts/{AccountId}/standing-orders | **API available** | ✔ | ✔ | ✔ | ✔ |
| Get Account Product | /accounts/{AccountId}/product | **API available** | ✔ | ✔ | ✔ | ✔ |
| Get Account Scheduled Payments | /accounts/{AccountId}/scheduled-payments | **API available** | ❌ | ❌ | ✔ | ✔ |
| Get Account Offers | /accounts/{AccountId}/offers | **API available** | ❌ | ❌ | ✔ | ✔ |
| Card Holder Names | /accounts/{AccountId}/card-holder-names | **API available** | ❌ | ❌ | ✔ | ✔ |

## In-Scope Accounts Matrix

The following table shows the in-scope accounts supported for each API version:

| Account Type | v1.1 | v2.0 | v3.1 | v4.0 |
| --- | --- | --- | --- | --- |
| Current Accounts\* | ✔ | ✔ | ✔ | ✔ |
| Savings Accounts\* | ❌ | ❌ | ✔ | ✔ |
| Credit & Charge Cards\* | ❌ | ❌ | ✔ | ✔ |
| Currency Accounts\* \*\* | ❌ | ❌ | ✔ | ✔ |
| Travel Accounts\* | ❌ | ❌ | ✔ | ✔ |

\*Please see the [*In-Scope Accounts document*](https://www.bankofapis.com/articles/ob-in-scope-accounts/aisp) for a comprehensive list of supported accounts

\*\* NatWest currency accounts use SchemeName UK.NWB.CurrencyAccount

### Travel Accounts

This feature allows customers to consent to linking a Travel account and retrieving account and transaction information for that account.

Overview of the new Travel account:

- The NatWest Group current account and linked Travel account will appear to **share the same Sort Code and Account Number** in the API response, but the **Account ID differs for the two accounts.**
- Additionally, the current account is GBP denominated, while the linked Travel account is currently only available in EUR denomination. More currencies will be added in the future.
- For existing current account consents, customers need to consent to the new linked Travel account for this account to be included in the responses provided by our Account & Transaction API.
- Funds can only be added to travel accounts from the linked current account.

To learn more about our new Travel account, click here: [What is a Travel account?](https://www.natwest.com/support-centre/travel-and-travel-insurance/general/travel-account.html)

## Account and Transaction API Specificationv4.0.0OAS 3.0

Swagger for Account and Transaction API Specification

[Contact Service Desk](mailto:ServiceDesk@openbanking.org.uk)

[open-licence](https://www.openbanking.org.uk/open-licence)

Servers

## API Testing

To help you get up and running with your development and integration to our REST APIs, we have now made it easier for you to perform the API testing before going live.

You can:

- Download our swagger for sandbox
- Import the environment file alongside the Postman collection

These endpoints do not contain real-time production data. You can test out your application and ensure that it is working correctly in a non-production environment instead.

### Postman settings instructions

Go to **Preferences or Settings > General** menu:

- in **REQUEST** column, make sure that **SSL certificate verification** is set to OFF (or make sure that the OpenBanking Pre-Production Issuing CA is in your trusted store)
- in **HEADERS** column, make sure that **Automatically follow redirects** is set to ON