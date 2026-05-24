---
title: "Account and Transactions | Barclays API Exchange"
source: "https://developer.barclays.com/portal/documentation/dc72e132-2951-4378-bc52-38e35b1e3564.bdn"
author:
published:
created: 2026-05-23
description:
tags:
  - "clippings"
---
## Account and Transactions

## Retrieve account information for Barclays customers...To retrieve account information for Barclays customers

## Overview

The Barclays Bank Account and Transactions Open Banking API version 4 (v4), is aimed at enhancing the functionality, security, and customer experience related to financial data sharing.

The Barclays Account and Transactions Open Banking API version 4 enables third parties to securely retrieve Barclays customer account information and transaction data.

| Version and Environment | Supported Endpoints | Unsupported Endpoints (please note that the scope remains the same as v3) |
| --- | --- | --- |
| • 4.0   • Production | • [POST /account-access-consents](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/account-access-consents.html#post-account-access-consents)   • [GET /account-access-consents/{ConsentId}](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/account-access-consents.html#get-account-access-consentsconsentid)   • [DELETE /account-access-consents/{ConsentId}](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/account-access-consents.html#get-account-access-consentsconsentid)   • [GET /accounts/{AccountId}](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Accounts.html#get-accountsaccountid)   • [GET /accounts](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Accounts.html#get-accountsaccountid)   • [GET /accounts/{AccountId}/balances](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Balances.html#get-accountsaccountidbalances)   • [GET accounts/{AccountId}/transactions](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Transactions.html#get-accountsaccountidtransactions)   • [GET /accounts/{AccountId}/statements](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Statements.html#get-accountsaccountidstatements)   • [GET /accounts/{AccountId}/statements/{StatementId}](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Statements.html#get-accountsaccountidstatementsstatementid)   • [GET /accounts/{AccountId}/statements/{StatementId}/transactions](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Statements.html#get-accountsaccountidstatementsstatementidtransactions)   • [GET /accounts/{AccountId}/beneficiaries](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Beneficiaries.html#get-accountsaccountidbeneficiaries)   • [GET /accounts/{AccountId}/direct-debits](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/direct-debits.html#get-accountsaccountiddirect-debits) \*   • [GET /accounts/{AccountId}/standing-orders](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/standing-orders.html#get-accountsaccountidstanding-orders) \*   • [GET /accounts/{AccountId}/product](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Products.html#get-accountsaccountidproduct)   • [GET /accounts/{AccountId}/offers](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Offers.html#get-accountsaccountidoffers)   • [GET /accounts/{AccountId}/parties](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/Parties.html#get-accountsaccountidparties)   • [GET /accounts/{AccountId}/scheduled-payments](https://openbankinguk.github.io/read-write-api-site3/v4.0/resources-and-data-models/aisp/scheduled-payments.html#get-accountsaccountidscheduled-payments)      \*These endpoints are **not** supported for currency or non-sterling accounts | • GET /balances   • GET /transactions   • GET /beneficiaries   • GET /direct-debits   • GET /standing-orders   • GET /products   • GET /party   • GET /accounts/{AccountId}/party   • GET /offers   • GET /scheduled-payments   • GET /statements   • GET /accounts/{AccountId}/statements/{StatementId}/file   ◦ All return a HTTP 405 Response |

**The following table shows Endpoints available by customer type.**

| Endpoints | v4/ISO 20022 Enhancements | Personal incl. Premier | Wealth | Business Banking | Barclaycard | Barclays Corporate | Barclaycard Commercial Payments | Barclays Consumer Bank Europe |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accounts | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ |
| Products | ✘ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ |
| Transactions | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ |
| Balances | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ |
| Standing Orders | ✔ | ✔ | ✔ | ✔ | ✘ | ✔ | ✘ | ✘ |
| Scheduled Payments | ✘ | ✔ | ✔ | ✔ | ✘ | ✔ | ✘ | ✘ |
| Direct Debits | ✔ | ✔ | ✔ | ✔ | ✘ | ✔ | ✘ | ✘ |
| Beneficiaries | ✘ | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| Offers | ✘ | ✘ | ✘ | ✘ | ✔ | ✘ | ✘ | ✘ |
| Parties | ✘ | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| Statements | ✘ | ✘ | ✘ | ✘ | ✔ | ✘ | ✘ | ✘ |

### Change Log

| **Here's what you need to know about Barclays Bank Account and Transactions API version 4** |
| --- |
| • The Barclays Account and Transaction Open Banking API version 4 endpoints provides data consistent with version 3.1, enabling users to access familiar information while preparing for future enhancements |
| • Please note that the revised v4 ISO 20022 changes have been applied as per the Open Banking Read-Write API Profile - v4.0 and detailed below in section: v4/ISO 20022 Enhancements Details |
| • Please visit the [Open Banking Limited website](https://openbankinguk.github.io/read-write-api-site3/v4.0/profiles/account-and-transaction-api-profile.html) to learn more about Account and Transactions API version 4.0 |

#### V4/ISO 20022 Enhancements Details

| Endpoints | v4 / ISO 20022 Enhancements Details |
| --- | --- |
| **Accounts Access Consents**      Allows an AISP to create, retrieve, and delete account access consent resource with Barclays | • Endpoints -   ◦ POST /account-access-consents   ◦ GET /account-access-consents/{ConsentId}      • Amended enumerations to align with ISO20022 for " ***Status field*** ". (AISPs should expect the above Status when querying the above GET endpoint and Barclays response to the POST endpoint)   ◦ AWAU (previously known as " ***AwaitingAuthorisation*** " in v3)   ◦ AUTH (previously known as " ***Authorised*** " in v3)   ◦ RJCT (previously known as " ***Rejected*** " in v3)   ◦ CANC (previously known as " ***Revoked*** " in v3) |
| **Accounts**      Allows an AISP to retrieve full list of Accounts and related information the Payment Services User (PSU) has authorised to access | • Endpoint - GET /accounts/{AccountId}\]   • Amended field names within Account object to align with v4 for -   ◦ `AccountSubType` re-named to `AccountTypeCode` (v4)   ◦ `AccountType` re-named to `AccountCategory` (v4) |
| **Products**      Allows an AISP to retrieve Product details for one or all authorised accounts | • Endpoint - GET /accounts/{AccountId}/product   • No changes from the Barclays v3 implementation of the endpoint |
| **Transactions**      Allows an AISP to retrieve Transaction details for one or all authorised accounts | • Endpoint - GET /accounts/{AccountId}/transactions   • Amended v4 Enumerations to align with ISO20022 for - `Type` field within the Balance object   ◦ INFO (Information)   • Amended v4 Enumerations to align with ISO20022 for - `Status` field within Transactions object   ◦ BOOK (Booked)   ◦ PDNG (Pending)   Third Party Providers (TPPs) should expect to receive one of the v4 enumerations when requesting status and type information in relation to transactions |
| **Balances**      Allows an AISP to retrieve Balance details for one or all authorised accounts | • Endpoint - GET /accounts/{AccountId}/balances   • Amended v4 Enumerations to align with ISO20022 for - `Type` field within the Balance object.   • When a TPP calls the above endpoint, the following are possible enumerations that will be returned by Barclays for the Balance type of Credit Card Products TPPs should expect to receive one of the v4 enumerations when requesting status and type information in relation to transactions)   ◦ ITBD (Interim Booked)   ◦ ITAV (Interim Available)   ◦ OPBD (Opening Booked)   ◦ Credit (Credit Limit)   • When a TPP calls the above endpoint, the following enumeration will be returned by Barclays for the Balance type of Current Account Products   ◦ Expected (XPCD) |
| **Standing Orders**      Allows an AISP to retrieve Standing order details for one or all authorised accounts | • Endpoint - GET /accounts/{AccountId}/standing-orders   • Mandated Relation Information - the inclusion of ***MandatedRelatedInformation*** object to align with v4. The new fields below are implemented in line with the v4 specification and ISO20022 (TPPs can expect payload responses to include these fields if information is present)   ◦ `FinalPaymentDateTime`   ◦ `FirstPaymentDateTime`   ◦ `RecurringPaymentDateTime`   ◦ `Frequency/Type` (See [Important Notes](#important-notes) for further information on the supported "frequency type" list)      • Remittance Information - the Inclusion of `RemittanceInformation` to align with v4. The fields below are implemented under this object in line with the v4 Specification and ISO20022 TPPs should not expect payload responses to include these fields)   ◦ `Unstructured`   ◦ `/Structured/AdditionalRemittanceInformation`   ◦ `/Structured/Invoicee`   ◦ `/Structured/Invoicer`   ◦ `/Structured/ReferredDocumentamount`   ◦ `/Structured/TaxRemittance`   ◦ `/Structured/CreditorReferenceInformation/Reference`      • The fields below are ***not*** implemented (TPPs should not expect payload responses to include these fields in the ***MandatedRelatedInformation*** object)   ◦ `CategoryPurposeCode`   ◦ `Classification`   ◦ `Reason`   ◦ `MandateIdentification`   ◦ `/Frequency/PointInTime`   ◦ `/Frequency/CounterPerPeriod` |
| **Direct Debits**      Allows an AISP to retrieve Direct Debit details for one or all authorised accounts | • Endpoint - GET /accounts/{AccountId}/direct-debits   • The Inclusion of ***MandatedRelatedInformation*** to align with v4. The fields below are implemented in line with the v4 Specification and ISO20022   ◦ `CategoryPurposeCode`   ◦ `Classification`   ◦ `FinalPaymentDateTime`   ◦ `FirstPaymentDateTime`   ◦ `MandateIdentification`   ◦ `Reason`   ◦ `RecurringPaymentDateTime`   ◦ `/Frequency/CountPerPeriod`   ◦ `/Frequency/PointInTime`   ◦ `/Frequency/Type` (will return a value of "NONE") |
| **Scheduled Payments**      Allows an AISP to retrieve Scheduled Payment details for one or all authorised accounts | • Endpoint - GET /accounts/{AccountId}/scheduled-payments   • No changes from the Barclays v3 implementation of the endpoint |
| **Beneficiaries**      Allows an AISP to retrieve Beneficiary details for one or all authorised accounts | • Endpoint - GET /accounts/{AccountId}/beneficiaries   • No changes from the Barclays v3 implementation of the endpoint |
| **Offers**      Allows an AISP to retrieve Offer details for one or all authorised accounts | • Endpoint - GET /accounts/{AccountId}/offers   • No changes from the Barclays v3 implementation of the endpoint |
| **Parties**      Allows an AISP to retrieve Parties details for one or all authorised accounts | • Endpoint - GET /accounts/{AccountId}/parties   • No changes from the Barclays v3 implementation of the endpoint |
| **Statements**      Allows an AISP to retrieve Statement details for one or all authorised accounts | Endpoints -   • GET /accounts/{AccountId}/statements   • GET /accounts/{AccountId}/statements/{StatementId}   • GET /accounts/{AccountId}/statements/{StatementId}/transactions   No changes from the Barclays v3 implementation of the endpoint |

[API docs by Redocly](https://redocly.com/redoc/)

## Account and Transactions (v4.0)

Download OpenAPI specification:[Download](blob:https://developer.barclays.com/b1ae6f32-9124-453b-b958-bf00aa1bebc2)

To retrieve account information for Barclays customers

## Account Access

Account Access

## Create Account Access Consents

Create Account Access Consents

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

##### Request Body schema: application/jsonrequired

Default

| required | object |
| --- | --- |
| required | object (OBRisk2)  The Risk section is sent by the initiating party to the ASPSP.   It is used to specify additional details for risk scoring for Account Info. |

### Responses

### Request samples

- Payload
Content type

application/json

`{ - "Data": { 	- "Permissions": [ 		- "ReadAccountsDetail", 		- "ReadBalances", 		- "ReadBeneficiariesDetail", 		- "ReadDirectDebits", 		- "ReadProducts", 		- "ReadStandingOrdersDetail", 		- "ReadTransactionsCredits", 		- "ReadTransactionsDebits", 		- "ReadTransactionsDetail", 		- "ReadOffers", 		- "ReadPAN", 		- "ReadParty", 		- "ReadPartyPSU", 		- "ReadScheduledPaymentsDetail", 		- "ReadStatementsDetail" 		], 	- "ExpirationDateTime": "2017-05-02T00:00:00+00:00", 	- "TransactionFromDateTime": "2017-05-03T00:00:00+00:00", 	- "TransactionToDateTime": "2017-12-03T00:00:00+00:00" 	}, - "Risk": { } }`

### Response samples

- 201
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "ConsentId": "urn-alphabank-intent-88379", 	- "Status": "AWAU", 	- "StatusUpdateDateTime": "2017-05-02T00:00:00+00:00", 	- "CreationDateTime": "2017-05-02T00:00:00+00:00", 	- "StatusReason": [ 		- { 			- "StatusReasonCode": "U036", 			- "StatusReasonDescription": "Waiting for completion of consent authorisation to be completed by user" 			} 		], 	- "Permissions": [ 		- "ReadAccountsDetail", 		- "ReadBalances", 		- "ReadBeneficiariesDetail", 		- "ReadDirectDebits", 		- "ReadProducts", 		- "ReadStandingOrdersDetail", 		- "ReadTransactionsCredits", 		- "ReadTransactionsDebits", 		- "ReadTransactionsDetail", 		- "ReadOffers", 		- "ReadPAN", 		- "ReadParty", 		- "ReadPartyPSU", 		- "ReadScheduledPaymentsDetail", 		- "ReadStatementsDetail" 		], 	- "ExpirationDateTime": "2017-08-02T00:00:00+00:00", 	- "TransactionFromDateTime": "2017-05-03T00:00:00+00:00", 	- "TransactionToDateTime": "2017-12-03T00:00:00+00:00" 	}, - "Risk": { }, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/account-access-consents/urn-alphabank-intent-88379" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Account Access Consents

Get Account Access Consents

##### path Parameters

| consentId  required | string \[ 1.. 25 \] characters ^BARCLAYS-\[A\]-\\d{14}$  Example: BARCLAYS-A-12345678901234  ConsentId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "ConsentId": "urn-alphabank-intent-88379", 	- "Status": "AWAU", 	- "StatusReason": [ 		- { 			- "StatusReasonCode": "U036", 			- "StatusReasonDescription": "Waiting for completion of consent authorisation to be completed by user" 			} 		], 	- "StatusReasonDescription": "Waiting for completion of consent authorisation to be completed by user", 	- "StatusUpdateDateTime": "2017-05-02T00:00:00+00:00", 	- "CreationDateTime": "2017-05-02T00:00:00+00:00", 	- "Permissions": [ 		- "ReadAccountsDetail", 		- "ReadBalances", 		- "ReadBeneficiariesDetail", 		- "ReadDirectDebits", 		- "ReadProducts", 		- "ReadStandingOrdersDetail", 		- "ReadTransactionsCredits", 		- "ReadTransactionsDebits", 		- "ReadTransactionsDetail", 		- "ReadOffers", 		- "ReadPAN", 		- "ReadParty", 		- "ReadPartyPSU", 		- "ReadScheduledPaymentsDetail", 		- "ReadStatementsDetail" 		], 	- "ExpirationDateTime": "2017-08-02T00:00:00+00:00", 	- "TransactionFromDateTime": "2017-05-03T00:00:00+00:00", 	- "TransactionToDateTime": "2017-12-03T00:00:00+00:00" 	}, - "Risk": { }, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/account-access-consents/urn-alphabank-intent-88379" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Delete Account Access Consents

Delete Account Access Consents

##### path Parameters

| consentId  required | string \[ 1.. 25 \] characters ^BARCLAYS-\[A\]-\\d{14}$  Example: BARCLAYS-A-12345678901234  ConsentId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 400
- 403
- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Accounts

Get Accounts

## Get Accounts

Get Accounts

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Account": [ 		- { 			- "AccountId": "22289", 			- "Status": "Enabled", 			- "MaturityDate": "2019-01-01T06:06:06+00:00", 			- "StatusUpdateDateTime": "2019-01-01T06:06:06+00:00", 			- "Currency": "GBP", 			- "AccountCategory": "Personal", 			- "AccountTypeCode": "CACC", 			- "Description": "For paying bills", 			- "Nickname": "Bills", 			- "OpeningDate": "2017-04-05T10:43:07+00:00", 			- "SwitchStatus": "UK.CASS.NotSwitched", 			- "StatementFrequencyAndFormat": [ 				- { 					- "Frequency": "YEAR", 					- "CommunicationMethod": "EMAL", 					- "Format": "DPDF", 					- "DeliveryAddress": { 						- "AddressType": "HOME", 						- "StreetName": "Bank Street", 						- "BuildingNumber": "11", 						- "Floor": "6", 						- "PostCode": "Z78 4TY", 						- "TownName": "London", 						- "Country": "UK" 						} 					} 				], 			- "Servicer": { 				- "SchemeName": "UK.OBIE.BICFI", 				- "Identification": "8020441910203345", 				- "Name": "ServicerName" 				}, 			- "Account": [ 				- { 					- "SchemeName": "UK.OBIE.SortCodeAccountNumber", 					- "Identification": "80200110203345", 					- "Name": "Mr Kevin", 					- "SecondaryIdentification": "00021", 					- "LEI": "9193001QZMP2PQT4AK86" 					} 				] 			}, 		- { 			- "AccountId": "31820", 			- "Status": "Enabled", 			- "StatusUpdateDateTime": "2018-01-01T06:06:06+00:00", 			- "Currency": "GBP", 			- "AccountCategory": "Personal", 			- "AccountTypeCode": "CACC", 			- "Nickname": "Household", 			- "Account": [ 				- { 					- "SchemeName": "UK.OBIE.SortCodeAccountNumber", 					- "Identification": "80200110203348", 					- "Name": "Mr Kevin" 					} 				] 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Accounts

Get Accounts

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Account": [ 		- { 			- "AccountId": "22289", 			- "Status": "Enabled", 			- "MaturityDate": "2019-01-01T06:06:06+00:00", 			- "StatusUpdateDateTime": "2019-01-01T06:06:06+00:00", 			- "Currency": "GBP", 			- "AccountCategory": "Personal", 			- "AccountTypeCode": "CACC", 			- "Description": "For paying bills", 			- "Nickname": "Bills", 			- "OpeningDate": "2017-04-05T10:43:07+00:00", 			- "SwitchStatus": "UK.CASS.NotSwitched", 			- "StatementFrequencyAndFormat": [ 				- { 					- "Frequency": "YEAR", 					- "CommunicationMethod": "EMAL", 					- "Format": "DPDF", 					- "DeliveryAddress": { 						- "AddressType": "HOME", 						- "StreetName": "Bank Street", 						- "BuildingNumber": "11", 						- "Floor": "6", 						- "PostCode": "Z78 4TY", 						- "TownName": "London", 						- "Country": "UK" 						} 					} 				], 			- "Servicer": { 				- "SchemeName": "UK.OBIE.BICFI", 				- "Identification": "8020441910203345", 				- "Name": "ServicerName" 				}, 			- "Account": [ 				- { 					- "SchemeName": "UK.OBIE.SortCodeAccountNumber", 					- "Identification": "80200110203345", 					- "Name": "Mr Kevin", 					- "SecondaryIdentification": "00021", 					- "LEI": "9193001QZMP2PQT4AK86" 					} 				] 			}, 		- { 			- "AccountId": "31820", 			- "Status": "Enabled", 			- "StatusUpdateDateTime": "2018-01-01T06:06:06+00:00", 			- "Currency": "GBP", 			- "AccountCategory": "Personal", 			- "AccountTypeCode": "CACC", 			- "Nickname": "Household", 			- "Account": [ 				- { 					- "SchemeName": "UK.OBIE.SortCodeAccountNumber", 					- "Identification": "80200110203348", 					- "Name": "Mr Kevin" 					} 				] 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Balances

Get Balances

## Get Balances

Get Balances

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Balance": [ 		- { 			- "AccountId": "22289", 			- "Amount": { 				- "Amount": "300.00", 				- "Currency": "GBP", 				- "SubType": "BCUR" 				}, 			- "LocalAmount": { 				- "Amount": "1230.00", 				- "Currency": "GBP", 				- "SubType": "BCUR" 				}, 			- "TotalAmount": { 				- "Amount": "1230.00", 				- "Currency": "GBP" 				}, 			- "CreditDebitIndicator": "Credit", 			- "Type": "ITAV", 			- "DateTime": "2017-04-05T10:43:07+00:00", 			- "CreditLine": [ 				- { 					- "Included": false, 					- "Amount": { 						- "Amount": "500.00", 						- "Currency": "GBP" 						}, 					- "Type": "Available" 					}, 				- { 					- "Included": false, 					- "Amount": { 						- "Amount": "500.00", 						- "Currency": "GBP" 						}, 					- "Type": "Pre-Agreed" 					} 				] 			} 		], 	- "TotalValue": { 		- "Amount": "720.39", 		- "Currency": "GBP" 		} 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/balances/" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Balances

Get Balances

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Beneficiaries

Get Beneficiaries

## Get Beneficiaries

Get Beneficiaries

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Beneficiary": [ 		- { 			- "AccountId": "22289", 			- "BeneficiaryId": "Ben1", 			- "BeneficiaryType": "Ordinary", 			- "Reference": "Towbar Club", 			- "CreditorAgent": { 				- "LEI": "IZ9Q00LZEVUKWCQY6X15", 				- "SchemeName": "UK.OBIE.BICFI", 				- "Identification": "80200112344562", 				- "Name": "The Credit Agent", 				- "PostalAddress": { 					- "AddressType": "BIZZ", 					- "StreetName": "Bank Street", 					- "BuildingNumber": "11", 					- "Floor": "6", 					- "PostCode": "Z78 4TY", 					- "TownName": "London", 					- "Country": "UK" 					} 				}, 			- "CreditorAccount": { 				- "SchemeName": "UK.OBIE.SortCodeAccountNumber", 				- "Identification": "80200112345678", 				- "SecondaryIdentification": "ID_0002", 				- "Name": "Mrs Juniper", 				- "Proxy": { 					- "Identification": "2360549017905188", 					- "Code": "TELE" 					} 				} 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/beneficiaries/" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Beneficiaries

Get Beneficiaries

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Direct Debits

Get Direct Debits

## Get Direct Debits

Get Direct Debits

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "DirectDebit": [ 		- { 			- "AccountId": "22289", 			- "DirectDebitId": "DD03", 			- "MandateRelatedInformation": { 				- "MandateIdentification": "Caravanners", 				- "Classification": "FIXE", 				- "CategoryPurposeCode": "BONU", 				- "FirstPaymentDateTime": "2024-04-25T12:46:49.425Z", 				- "RecurringPaymentDateTime": "2024-04-25T12:46:49.425Z", 				- "FinalPaymentDateTime": "2024-04-25T12:46:49.425Z", 				- "Reason": "To pay monthly membership", 				- "Frequency": { 					- "Type": "WEEK", 					- "CountPerPeriod": 1 					} 				}, 			- "DirectDebitStatusCode": "ACTV", 			- "Name": "Towbar Club 3 - We Love Towbars", 			- "PreviousPaymentDateTime": "2017-04-05T10:43:07+00:00", 			- "PreviousPaymentAmount": { 				- "Amount": "0.57", 				- "Currency": "GBP" 				} 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/direct-debits/" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Direct Debits

Get Direct Debits

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Offers

Get Offers

## Get Offers

Get Offers

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Offer": [ 		- { 			- "AccountId": "22289", 			- "OfferId": "Offer1", 			- "OfferType": "LimitIncrease", 			- "Description": "Credit limit increase for the account up to £10000.00", 			- "StartDateTime": "2024-05-29T00:00:00Z", 			- "EndDateime": "2024-06-29T00:00:00Z", 			- "Rate": "100.00", 			- "Value": 10, 			- "Term": "Starting first of the month and ending at the end of year", 			- "Fee": { 				- "Amount": "01.50", 				- "Currency": "GBP" 				}, 			- "URL": "http://modelbank.com/offer/offer1", 			- "Amount": { 				- "Amount": "10000.00", 				- "Currency": "GBP" 				} 			}, 		- { 			- "AccountId": "22289", 			- "OfferId": "Offer2", 			- "OfferType": "BalanceTransfer", 			- "StartDateTime": "2024-05-29T00:00:00Z", 			- "EndDateTime": "2024-06-29T00:00:00Z", 			- "Description": "Balance transfer offer up to £2000", 			- "Amount": { 				- "Amount": "2000.00", 				- "Currency": "GBP" 				} 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/offers/" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Offers

Get Offers

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Parties

Get Parties

## Get Parties

Get Parties

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Party": [ 		- { 			- "PartyId": "PABC123", 			- "PartyType": "Sole", 			- "Name": "Semiotec", 			- "FullLegalName": "Semiotec Limited", 			- "LegalStructure": "UK.OBIE.PrivateLimitedCompany", 			- "BeneficialOwnership": true, 			- "AccountRole": "UK.OBIE.Principal", 			- "EmailAddress": "contact@semiotec.co.jp", 			- "LEI": "068700IA8DVYPS77MD05", 			- "Phone": "+44-2079460000", 			- "Mobile": "+44-7700900000", 			- "Address": [ 				- { 					- "AddressType": "BIZZ", 					- "StreetName": "Street", 					- "BuildingNumber": "15", 					- "PostCode": "NW1 1AB", 					- "TownName": "London", 					- "Country": "GB" 					} 				] 			}, 		- { 			- "PartyId": "PXSIF023", 			- "PartyNumber": "0000007456", 			- "PartyType": "Delegate", 			- "Name": "Kevin Atkinson", 			- "FullLegalName": "Mr Kevin Bartholmew Atkinson", 			- "LegalStructure": "UK.OBIE.Individual", 			- "BeneficialOwnership": false, 			- "LEI": "068700IA8DVHGY77MD85", 			- "AccountRole": "UK.OBIE.Administrator", 			- "EmailAddress": "kev@semiotec.co.jp", 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/parties" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Parties

Get Parties

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Get Parties

Get Parties

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Products

Get Products

## Get Products

Get Products

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Product": [ 		- { 			- "AccountId": "22289", 			- "ProductId": "51B", 			- "SecondaryProductId": "CA78", 			- "MarketingStateId": "22878123", 			- "ProductType": "PersonalCurrentAccount", 			- "ProductName": "321 Product", 			- "PCA": { }, 			- "BCA": { } 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/product" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Products

Get Products

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Scheduled Payments

Get Scheduled Payments

## Get Scheduled Payments

Get Scheduled Payments

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Product": [ 		- { 			- "AccountId": "22289", 			- "ProductId": "51B", 			- "SecondaryProductId": "CA78", 			- "MarketingStateId": "22878123", 			- "ProductType": "PersonalCurrentAccount", 			- "ProductName": "321 Product", 			- "PCA": { }, 			- "BCA": { } 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/product" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Scheduled Payments

Get Scheduled Payments

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Standing Orders

Get Standing Orders

## Get Standing Orders

Get Standing Orders

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "StandingOrder": [ 		- { 			- "AccountId": "22289", 			- "StandingOrderId": "Ben3", 			- "FirstPaymentAmount": { 				- "Amount": "0.57", 				- "Currency": "GBP" 				}, 			- "NextPaymentDateTime": "2017-08-13T00:00:00+00:00", 			- "NextPaymentAmount": { 				- "Amount": "0.56", 				- "Currency": "GBP" 				}, 			- "FinalPaymentAmount": { 				- "Amount": "0.56", 				- "Currency": "GBP" 				}, 			- "LastPaymentDateTime": "2017-07-13T00:00:00+00:00", 			- "LastPaymentAmount": { 				- "Amount": "0.56", 				- "Currency": "GBP" 				}, 			- "CreditorAgent": { 				- "SchemeName": "UK.OBIE.BICFI", 				- "Identification": "80200112344562" 				}, 			- "MandateRelatedInformation": { 				- "MandateIdentification": "Golfers", 				- "Classification": "FIXE", 				- "CategoryPurposeCode": "BONU", 				- "FirstPaymentDateTime": "2024-04-25T12:46:49.425Z", 				- "RecurringPaymentDateTime": "2024-04-25T12:46:49.425Z", 				- "FinalPaymentDateTime": "2024-04-25T12:46:49.425Z", 				- "Reason": "To pay monthly membership", 				- "Frequency": { 					- "Type": "MNTH", 					- "CountPerPeriod": 1 					} 				}, 			- "StandingOrderStatusCode": "ACTV", 			- "CreditorAccount": { 				- "SchemeName": "UK.OBIE.SortCodeAccountNumber", 				- "Identification": "80200112345678", 				- "SecondaryIdentification": "80200112895462", 				- "Name": "Mrs Juniper", 				- "Proxy": { 					- "Identification": "441234012345", 					- "Code": "TELE", 					- "Type": "Telephone" 					} 				} 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/standing-orders/" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Standing Orders

Get Standing Orders

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Statements

Get Statements

## Get Statements

Get Statements

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### query Parameters

| fromStatementDateTime | string  Example: fromStatementDateTime=2017-04-05T10:43:07+00:00  The UTC ISO 8601 Date Time to filter statements FROM NB Time component is optional - set to 00:00:00 for just Date. If the Date Time contains a timezone, the ASPSP must ignore the timezone component. |
| --- | --- |
| toStatementDateTime | string  Example: toStatementDateTime=2017-04-05T10:43:07+00:00  The UTC ISO 8601 Date Time to filter statements TO NB Time component is optional - set to 00:00:00 for just Date. If the Date Time contains a timezone, the ASPSP must ignore the timezone component. |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Statement": [ 		- { 			- "AccountId": "22289", 			- "StatementId": "8sfhke-sifhkeuf-97813", 			- "StatementReference": "002", 			- "Type": "RegularPeriodic", 			- "StartDateTime": "2017-08-01T00:00:00+00:00", 			- "EndDateTime": "2017-08-31T23:59:59+00:00", 			- "CreationDateTime": "2017-09-01T00:00:00+00:00", 			- "StatementDateTime": [ 				- { 					- "DateTime": "2017-08-01T00:00:00+00:00", 					- "Type": "UK.OBIE.DirectDebitDue" 					} 				], 			- "TotalValue": { 				- "Amount": "1024.00", 				- "Currency": "GBP" 				}, 			- "StatementDescription": [ 				- "August 2017 Statement", 				- "One Free Uber Ride" 				], 			- "StatementAmount": [ 				- { 					- "Amount": { 						- "Amount": "400.00", 						- "Currency": "GBP", 						- "SubType": "BCUR" 						}, 					- "LocalAmount": { 						- "Amount": "400.00", 						- "Currency": "GBP", 						- "SubType": "BCUR" 						}, 					- "CreditDebitIndicator": "Credit", 					- "Type": "ClosingBalance" 					}, 				- { 					- "Amount": { 						- "Amount": "600.00", 						- "Currency": "GBP" 						}, 					- "CreditDebitIndicator": "Credit", 					- "Type": "PreviousClosingBalance" 					} 				], 			- "StatementBenefit": [ 				- { 					- "Type": "UK.OBIE.Cashback", 					- "Amount": { 						- "Amount": "5.00", 						- "Currency": "GBP" 						} 					} 				], 			- "StatementValue": [ 				- { 					- "Type": "UK.OBIE.AirMilesPoints", 					- "Value": "100" 					} 				], 			- "StatementFee": [ 				- { 					- "Description": "International usage charge", 					- "Type": "UK.OBIE.ForeignTransaction", 					- "Rate": 0.229, 					- "CreditDebitIndicator": "Credit", 					- "RateType": "UK.OBIE.AER", 					- "Frequency": "UK.OBIE.StatementMonthly", 					- "Amount": { 						- "Amount": "03.75", 						- "Currency": "GBP" 						} 					} 				], 			- "StatementInterest": [ 				- { 					- "Description": "Interest occurred over statement duration", 					- "Type": "UK.OBIE.Total", 					- "Rate": 0.229, 					- "CreditDebitIndicator": "Credit", 					- "RateType": "UK.OBIE.FixedRate", 					- "Frequency": "UK.OBIE.StatementMonthly", 					- "Amount": { 						- "Amount": "20.25", 						- "Currency": "GBP" 						} 					} 				], 			- "StatementRate": [ 				- { 					- "Rate": "0.229", 					- "Type": "UK.OBIE.MonthlyPurchase" 					} 				] 			}, 		- { 			- "AccountId": "22289", 			- "StatementId": "34hj24u-324h33-31i3p4", 			- "StatementReference": "003", 			- "Type": "RegularPeriodic", 			- "StartDateTime": "2017-09-01T00:00:00+00:00", 			- "EndDateTime": "2017-09-30T23:59:59+00:00", 			- "CreationDateTime": "2017-10-01T00:00:00+00:00", 			- "StatementDescription": [ 				- "September 2017 Statement" 				], 			- "StatementAmount": [ 				- { 					- "Amount": { 						- "Amount": "200.00", 						- "Currency": "GBP" 						}, 					- "CreditDebitIndicator": "Credit", 					- "Type": "PreviousClosingBalance" 					}, 				- { 					- "Amount": { 						- "Amount": "400.00", 						- "Currency": "GBP" 						}, 					- "CreditDebitIndicator": "Credit", 					- "Type": "PreviousClosingBalance" 					} 				] 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/statements/" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Statements

Get Statements

##### path Parameters

| statementId  required | string \[ 1.. 40 \] characters  Example: 1234567890  StatementId |
| --- | --- |
| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Statement": [ 		- { 			- "AccountId": "22289", 			- "StatementId": "8sfhke-sifhkeuf-97813", 			- "StatementReference": "002", 			- "Type": "RegularPeriodic", 			- "StartDateTime": "2017-08-01T00:00:00+00:00", 			- "EndDateTime": "2017-08-31T23:59:59+00:00", 			- "CreationDateTime": "2017-09-01T00:00:00+00:00", 			- "StatementDateTime": [ 				- { 					- "DateTime": "2017-08-01T00:00:00+00:00", 					- "Type": "UK.OBIE.DirectDebitDue" 					} 				], 			- "TotalValue": { 				- "Amount": "1024.00", 				- "Currency": "GBP" 				}, 			- "StatementDescription": [ 				- "August 2017 Statement", 				- "One Free Uber Ride" 				], 			- "StatementAmount": [ 				- { 					- "Amount": { 						- "Amount": "400.00", 						- "Currency": "GBP", 						- "SubType": "BCUR" 						}, 					- "LocalAmount": { 						- "Amount": "400.00", 						- "Currency": "GBP", 						- "SubType": "BCUR" 						}, 					- "CreditDebitIndicator": "Credit", 					- "Type": "ClosingBalance" 					}, 				- { 					- "Amount": { 						- "Amount": "600.00", 						- "Currency": "GBP" 						}, 					- "CreditDebitIndicator": "Credit", 					- "Type": "PreviousClosingBalance" 					} 				], 			- "StatementBenefit": [ 				- { 					- "Type": "UK.OBIE.Cashback", 					- "Amount": { 						- "Amount": "5.00", 						- "Currency": "GBP" 						} 					} 				], 			- "StatementValue": [ 				- { 					- "Type": "UK.OBIE.AirMilesPoints", 					- "Value": "100" 					} 				], 			- "StatementFee": [ 				- { 					- "Description": "International usage charge", 					- "Type": "UK.OBIE.ForeignTransaction", 					- "Rate": 0.229, 					- "CreditDebitIndicator": "Credit", 					- "RateType": "UK.OBIE.AER", 					- "Frequency": "UK.OBIE.StatementMonthly", 					- "Amount": { 						- "Amount": "03.75", 						- "Currency": "GBP" 						} 					} 				], 			- "StatementInterest": [ 				- { 					- "Description": "Interest occurred over statement duration", 					- "Type": "UK.OBIE.Total", 					- "Rate": 0.229, 					- "CreditDebitIndicator": "Credit", 					- "RateType": "UK.OBIE.FixedRate", 					- "Frequency": "UK.OBIE.StatementMonthly", 					- "Amount": { 						- "Amount": "20.25", 						- "Currency": "GBP" 						} 					} 				], 			- "StatementRate": [ 				- { 					- "Rate": "0.229", 					- "Type": "UK.OBIE.MonthlyPurchase" 					} 				] 			}, 		- { 			- "AccountId": "22289", 			- "StatementId": "34hj24u-324h33-31i3p4", 			- "StatementReference": "003", 			- "Type": "RegularPeriodic", 			- "StartDateTime": "2017-09-01T00:00:00+00:00", 			- "EndDateTime": "2017-09-30T23:59:59+00:00", 			- "CreationDateTime": "2017-10-01T00:00:00+00:00", 			- "StatementDescription": [ 				- "September 2017 Statement" 				], 			- "StatementAmount": [ 				- { 					- "Amount": { 						- "Amount": "200.00", 						- "Currency": "GBP" 						}, 					- "CreditDebitIndicator": "Credit", 					- "Type": "PreviousClosingBalance" 					}, 				- { 					- "Amount": { 						- "Amount": "400.00", 						- "Currency": "GBP" 						}, 					- "CreditDebitIndicator": "Credit", 					- "Type": "PreviousClosingBalance" 					} 				] 			} 		] 	}, - "Links": { 	- "Self": "https://api.alphabank.com/open-banking/v4.0/aisp/accounts/22289/statements/" 	}, - "Meta": { 	- "TotalPages": 1 	} }`

## Get Statements

Get Statements

##### path Parameters

| statementId  required | string \[ 1.. 40 \] characters  Example: 1234567890  StatementId |
| --- | --- |
| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Get Statements

Get Statements

##### query Parameters

| fromStatementDateTime | string  Example: fromStatementDateTime=2017-04-05T10:43:07+00:00  The UTC ISO 8601 Date Time to filter statements FROM NB Time component is optional - set to 00:00:00 for just Date. If the Date Time contains a timezone, the ASPSP must ignore the timezone component. |
| --- | --- |
| toStatementDateTime | string  Example: toStatementDateTime=2017-04-05T10:43:07+00:00  The UTC ISO 8601 Date Time to filter statements TO NB Time component is optional - set to 00:00:00 for just Date. If the Date Time contains a timezone, the ASPSP must ignore the timezone component. |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

## Transactions

Get Transactions

## Get Transactions

Get Transactions

##### path Parameters

| statementId  required | string \[ 1.. 40 \] characters  Example: 1234567890  StatementId |
| --- | --- |
| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Transaction": [ 		- { 			- "AccountId": "22289", 			- "TransactionId": "string", 			- "TransactionReference": "string", 			- "StatementReference": [ 				- "002" 				], 			- "CreditDebitIndicator": "Credit", 			- "Status": "BOOK", 			- "TransactionMutability": "Mutable", 			- "BookingDateTime": "2019-08-24T14:15:22Z", 			- "ValueDateTime": "2019-08-24T14:15:22Z", 			- "TransactionInformation": "string", 			- "AddressLine": "string", 			- "Amount": { 				- "Amount": "1209.06", 				- "Currency": "GBP" 				}, 			- "ChargeAmount": { 				- "Amount": "1209.06", 				- "Currency": "GBP" 				}, 			- "CurrencyExchange": { 				- "SourceCurrency": "string", 				- "TargetCurrency": "string", 				- "UnitCurrency": "string", 				- "ExchangeRate": 0, 				- "ContractIdentification": "string", 				- "QuotationDate": "2019-08-24T14:15:22Z", 				- "InstructedAmount": { 					- "Amount": "1209.06", 					- "Currency": "GBP" 					} 				}, 			- "BankTransactionCode": { 				- "Code": "string", 				- "SubCode": "string" 				}, 			- "ProprietaryBankTransactionCode": { 				- "Code": "string", 				- "Issuer": "string" 				}, 			- "ExtendedProprietaryBankTransactionCodes": [ 				- { 					- "Code": "string", 					- "Issuer": "string", 					- "Description": "string" 					} 				], 			- "Balance": { 				- "CreditDebitIndicator": "Credit", 				- "Type": "CLAV", 				- "Amount": { 					- "Amount": "1209.06", 					- "Currency": "GBP" 					} 				}, 			- "MerchantDetails": { 				- "MerchantName": "string", 				- "MerchantCategoryCode": "stri" 				}, 			- "CreditorAgent": { 				- "SchemeName": "UK.OBIE.BICFI", 				- "Identification": "string", 				- "Name": "Agent Name", 				- "LEI": "IZ9Q00LZEVUKWCQY6X15", 				- "PostalAddress": { 					- "AddressType": "BIZZ", 					- "Department": "Finance", 					- "SubDepartment": "Payroll", 					- "StreetName": "Bank Street", 					- "BuildingNumber": "11", 					- "BuildingName": "string", 					- "Floor": "11", 					- "UnitNumber": "A88", 					- "Room": "Basement 03", 					- "PostBox": "PO Box 123456", 					- "TownLocationName": "London", 					- "DistrictName": "Greater London", 					- "CareOf": "Jane Smith", 					- "PostCode": "EC2N 4AG", 					- "TownName": "London", 					- "CountrySubDivision": "string", 					- "Country": "string", 					- "AddressLine": [ 						- "string" 						] 					} 				}, 			- "CreditorAccount": { 				- "SchemeName": "string", 				- "Identification": "80200112344562", 				- "Name": "Jane Smith", 				- "SecondaryIdentification": "87562298675897", 				- "Proxy": { 					- "Identification": "2360549017905188", 					- "Code": "TELE", 					- "Type": "string" 					} 				}, 			- "DebtorAgent": { 				- "SchemeName": "UK.OBIE.BICFI", 				- "Identification": "string", 				- "Name": "Agent Name", 				- "LEI": "IZ9Q00LZEVUKWCQY6X15", 				- "PostalAddress": { 					- "AddressType": "BIZZ", 					- "Department": "Finance", 					- "SubDepartment": "Payroll", 					- "StreetName": "Bank Street", 					- "BuildingNumber": "11", 					- "BuildingName": "string", 					- "Floor": "11", 					- "UnitNumber": "A88", 					- "Room": "Basement 03", 					- "PostBox": "PO Box 123456", 					- "TownLocationName": "London", 					- "DistrictName": "Greater London", 					- "CareOf": "Jane Smith", 					- "PostCode": "EC2N 4AG", 					- "TownName": "London", 					- "CountrySubDivision": "string", 					- "Country": "string", 					- "AddressLine": [ 						- "string" 						] 					} 				}, 			- "DebtorAccount": { 				- "SchemeName": "string", 				- "Identification": "80200112344562", 				- "Name": "Jane Smith", 				- "SecondaryIdentification": "87562298675897", 				- "Proxy": { 					- "Identification": "2360549017905188", 					- "Code": "TELE", 					- "Type": "string" 					} 				}, 			- "CardInstrument": { 				- "CardSchemeName": "AmericanExpress", 				- "AuthorisationType": "ConsumerDevice", 				- "Name": "string", 				- "Identification": "string" 				}, 			- "SupplementaryData": { }, 			- "CategoryPurposeCode": "BONU", 			- "PaymentPurposeCode": "BKDF", 			- "UltimateCreditor": { 				- "Name": "string", 				- "Identification": "string", 				- "LEI": "IZ9Q00LZEVUKWCQY6X15", 				- "SchemeName": "string", 				- "PostalAddress": { 					- "AddressType": "BIZZ", 					- "Department": "Finance", 					- "SubDepartment": "Payroll", 					- "StreetName": "Bank Street", 					- "BuildingNumber": "11", 					- "BuildingName": "string", 					- "Floor": "11", 					- "UnitNumber": "A88", 					- "Room": "Basement 03", 					- "PostBox": "PO Box 123456", 					- "TownLocationName": "London", 					- "DistrictName": "Greater London", 					- "CareOf": "Jane Smith", 					- "PostCode": "EC2N 4AG", 					- "TownName": "London", 					- "CountrySubDivision": "string", 					- "Country": "string", 					- "AddressLine": [ 						- "string" 						] 					} 				}, 			- "UltimateDebtor": { 				- "Name": "string", 				- "Identification": "string", 				- "LEI": "IZ9Q00LZEVUKWCQY6X15", 				- "SchemeName": "string", 				- "PostalAddress": { 					- "AddressType": "BIZZ", 					- "Department": "Finance", 					- "SubDepartment": "Payroll", 					- "StreetName": "Bank Street", 					- "BuildingNumber": "11", 					- "BuildingName": "string", 					- "Floor": "11", 					- "UnitNumber": "A88", 					- "Room": "Basement 03", 					- "PostBox": "PO Box 123456", 					- "TownLocationName": "London", 					- "DistrictName": "Greater London", 					- "CareOf": "Jane Smith", 					- "PostCode": "EC2N 4AG", 					- "TownName": "London", 					- "CountrySubDivision": "string", 					- "Country": "string", 					- "AddressLine": [ 						- "string" 						] 					} 				} 			} 		] 	}, - "Meta": { 	- "TotalPages": 42, 	- "FirstAvailableDateTime": "2019-08-24T14:15:22Z", 	- "LastAvailableDateTime": "2019-08-24T14:15:22Z" 	} }`

## Get Transactions

Get Transactions

##### path Parameters

| accountId  required | string \[ 1.. 20 \] characters ^\\d{1,20}$  Example: 12345678901234567890  AccountId |
| --- | --- |

##### query Parameters

| fromBookingDateTime | string  Example: fromBookingDateTime=2017-04-05T10:43:07+00:00  The UTC ISO 8601 Date Time to filter transactions FROM NB Time component is optional - set to 00:00:00 for just Date. If the Date Time contains a timezone, the ASPSP must ignore the timezone component. |
| --- | --- |
| toBookingDateTime | string  Example: toBookingDateTime=2017-04-05T10:43:07+00:00  The UTC ISO 8601 Date Time to filter transactions TO NB Time component is optional - set to 00:00:00 for just Date. If the Date Time contains a timezone, the ASPSP must ignore the timezone component. |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 200
- 400
- 403
- 500
Content type

application/json

`{ - "Data": { 	- "Transaction": [ 		- { 			- "AccountId": "22289", 			- "TransactionId": "string", 			- "TransactionReference": "string", 			- "StatementReference": [ 				- "002" 				], 			- "CreditDebitIndicator": "Credit", 			- "Status": "BOOK", 			- "TransactionMutability": "Mutable", 			- "BookingDateTime": "2019-08-24T14:15:22Z", 			- "ValueDateTime": "2019-08-24T14:15:22Z", 			- "TransactionInformation": "string", 			- "AddressLine": "string", 			- "Amount": { 				- "Amount": "1209.06", 				- "Currency": "GBP" 				}, 			- "ChargeAmount": { 				- "Amount": "1209.06", 				- "Currency": "GBP" 				}, 			- "CurrencyExchange": { 				- "SourceCurrency": "string", 				- "TargetCurrency": "string", 				- "UnitCurrency": "string", 				- "ExchangeRate": 0, 				- "ContractIdentification": "string", 				- "QuotationDate": "2019-08-24T14:15:22Z", 				- "InstructedAmount": { 					- "Amount": "1209.06", 					- "Currency": "GBP" 					} 				}, 			- "BankTransactionCode": { 				- "Code": "string", 				- "SubCode": "string" 				}, 			- "ProprietaryBankTransactionCode": { 				- "Code": "string", 				- "Issuer": "string" 				}, 			- "ExtendedProprietaryBankTransactionCodes": [ 				- { 					- "Code": "string", 					- "Issuer": "string", 					- "Description": "string" 					} 				], 			- "Balance": { 				- "CreditDebitIndicator": "Credit", 				- "Type": "CLAV", 				- "Amount": { 					- "Amount": "1209.06", 					- "Currency": "GBP" 					} 				}, 			- "MerchantDetails": { 				- "MerchantName": "string", 				- "MerchantCategoryCode": "stri" 				}, 			- "CreditorAgent": { 				- "SchemeName": "UK.OBIE.BICFI", 				- "Identification": "string", 				- "Name": "Agent Name", 				- "LEI": "IZ9Q00LZEVUKWCQY6X15", 				- "PostalAddress": { 					- "AddressType": "BIZZ", 					- "Department": "Finance", 					- "SubDepartment": "Payroll", 					- "StreetName": "Bank Street", 					- "BuildingNumber": "11", 					- "BuildingName": "string", 					- "Floor": "11", 					- "UnitNumber": "A88", 					- "Room": "Basement 03", 					- "PostBox": "PO Box 123456", 					- "TownLocationName": "London", 					- "DistrictName": "Greater London", 					- "CareOf": "Jane Smith", 					- "PostCode": "EC2N 4AG", 					- "TownName": "London", 					- "CountrySubDivision": "string", 					- "Country": "string", 					- "AddressLine": [ 						- "string" 						] 					} 				}, 			- "CreditorAccount": { 				- "SchemeName": "string", 				- "Identification": "80200112344562", 				- "Name": "Jane Smith", 				- "SecondaryIdentification": "87562298675897", 				- "Proxy": { 					- "Identification": "2360549017905188", 					- "Code": "TELE", 					- "Type": "string" 					} 				}, 			- "DebtorAgent": { 				- "SchemeName": "UK.OBIE.BICFI", 				- "Identification": "string", 				- "Name": "Agent Name", 				- "LEI": "IZ9Q00LZEVUKWCQY6X15", 				- "PostalAddress": { 					- "AddressType": "BIZZ", 					- "Department": "Finance", 					- "SubDepartment": "Payroll", 					- "StreetName": "Bank Street", 					- "BuildingNumber": "11", 					- "BuildingName": "string", 					- "Floor": "11", 					- "UnitNumber": "A88", 					- "Room": "Basement 03", 					- "PostBox": "PO Box 123456", 					- "TownLocationName": "London", 					- "DistrictName": "Greater London", 					- "CareOf": "Jane Smith", 					- "PostCode": "EC2N 4AG", 					- "TownName": "London", 					- "CountrySubDivision": "string", 					- "Country": "string", 					- "AddressLine": [ 						- "string" 						] 					} 				}, 			- "DebtorAccount": { 				- "SchemeName": "string", 				- "Identification": "80200112344562", 				- "Name": "Jane Smith", 				- "SecondaryIdentification": "87562298675897", 				- "Proxy": { 					- "Identification": "2360549017905188", 					- "Code": "TELE", 					- "Type": "string" 					} 				}, 			- "CardInstrument": { 				- "CardSchemeName": "AmericanExpress", 				- "AuthorisationType": "ConsumerDevice", 				- "Name": "string", 				- "Identification": "string" 				}, 			- "SupplementaryData": { }, 			- "CategoryPurposeCode": "BONU", 			- "PaymentPurposeCode": "BKDF", 			- "UltimateCreditor": { 				- "Name": "string", 				- "Identification": "string", 				- "LEI": "IZ9Q00LZEVUKWCQY6X15", 				- "SchemeName": "string", 				- "PostalAddress": { 					- "AddressType": "BIZZ", 					- "Department": "Finance", 					- "SubDepartment": "Payroll", 					- "StreetName": "Bank Street", 					- "BuildingNumber": "11", 					- "BuildingName": "string", 					- "Floor": "11", 					- "UnitNumber": "A88", 					- "Room": "Basement 03", 					- "PostBox": "PO Box 123456", 					- "TownLocationName": "London", 					- "DistrictName": "Greater London", 					- "CareOf": "Jane Smith", 					- "PostCode": "EC2N 4AG", 					- "TownName": "London", 					- "CountrySubDivision": "string", 					- "Country": "string", 					- "AddressLine": [ 						- "string" 						] 					} 				}, 			- "UltimateDebtor": { 				- "Name": "string", 				- "Identification": "string", 				- "LEI": "IZ9Q00LZEVUKWCQY6X15", 				- "SchemeName": "string", 				- "PostalAddress": { 					- "AddressType": "BIZZ", 					- "Department": "Finance", 					- "SubDepartment": "Payroll", 					- "StreetName": "Bank Street", 					- "BuildingNumber": "11", 					- "BuildingName": "string", 					- "Floor": "11", 					- "UnitNumber": "A88", 					- "Room": "Basement 03", 					- "PostBox": "PO Box 123456", 					- "TownLocationName": "London", 					- "DistrictName": "Greater London", 					- "CareOf": "Jane Smith", 					- "PostCode": "EC2N 4AG", 					- "TownName": "London", 					- "CountrySubDivision": "string", 					- "Country": "string", 					- "AddressLine": [ 						- "string" 						] 					} 				} 			} 		] 	}, - "Meta": { 	- "TotalPages": 42, 	- "FirstAvailableDateTime": "2019-08-24T14:15:22Z", 	- "LastAvailableDateTime": "2019-08-24T14:15:22Z" 	} }`

## Get Transactions

Get Transactions

##### query Parameters

| fromBookingDateTime | string  Example: fromBookingDateTime=2017-04-05T10:43:07+00:00  The UTC ISO 8601 Date Time to filter transactions FROM NB Time component is optional - set to 00:00:00 for just Date. If the Date Time contains a timezone, the ASPSP must ignore the timezone component. |
| --- | --- |
| toBookingDateTime | string  Example: toBookingDateTime=2017-04-05T10:43:07+00:00  The UTC ISO 8601 Date Time to filter transactions TO NB Time component is optional - set to 00:00:00 for just Date. If the Date Time contains a timezone, the ASPSP must ignore the timezone component. |

##### header Parameters

| x-fapi-auth-date | string = 29 characters ^(Mon\|Tue\|Wed\|Thu\|Fri\|Sat\|Sun), \\d{2} (Jan\|Fe...  Example: Sun, 10 Sep 2017 19:43:31 UTC  The time when the PSU last logged in with the TPP. All dates in the HTTP headers are represented as RFC 7231 Full Dates. An example is below: Sun, 10 Sep 2017 19:43:31 UTC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-fapi-customer-ip-address | string \[ 7.. 40 \] characters ^((?:(?:25\[0-5\]\|2\[0-4\]\[0-9\]\|\[01\]?\[0-9\]\[0-9\]?)...  Example: 104.25.212.99  The PSU's IP address if the PSU is currently logged in with the TPP. |
| x-fapi-interaction-id | string \[ 32.. 36 \] characters ^\[A-Za-f0-9\]{8}-?\[A-Za-f0-9\]{4}-?\[1-5\]\[A-Za-f...  Example: 93bac548-d2de-4546-b106-880a5018460d  An RFC4122 UID used as a correlation id. |
| Authorization  required | string \[ 1.. 4871 \] characters ^Bearer \[A-Za-z0-9-\_=\]{1,256}\\.\\.\[A-Za-z0-9-\_...  Example: Bearer eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0OTk4NTA5NjUsInN1YiI6IkJhcmNsYXlzX1BheW1lbnRfU2VydmljZSIsInNjb3BlIjpbImlkcy5tYW5hZ2Vfa2V5IiwiaWRzLm1hbmFnZV9jbGllbnQiXSwiaXNzIjoiaHR0cDovL2lkZW50aXR5LXNlcnZpY2UvIiwiaWF0IjoxNDk5ODUwMDY1fQ.OX-u14YLs7iksl6gnZ9ZqMBu-ekFi4pSva5mzhuf2xU  An Authorisation Token as per [https://tools.ietf.org/html/rfc6750](https://tools.ietf.org/html/rfc6750) |
| x-customer-user-agent | string \[ 1.. 500 \] characters  Example: Mozilla/5.0 (iPad; U; CPU OS 3\_2\_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405  Indicates the user-agent that the PSU is using. |

### Responses

### Response samples

- 500
Content type

application/json

`{ - "Code": "OB.BadRequest", - "Id": "2b5f0fb2-730b-11e8-adc0-fa7ae01bbebc", - "Message": "Invalid request parameters", - "Errors": [ 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version must be supplied", 		- "Path": "Data.Initiation", 		- "Url": "<url to the api reference for Event Notification API>" 		}, 	- { 		- "ErrorCode": "AC17", 		- "Message": "Version supplied is not valid", 		- "Path": "Data.Initiation.CreditorAccount", 		- "Url": "<url to the api reference for Event Notification API>" 		} 	] }`

[[Personal Finance System]]