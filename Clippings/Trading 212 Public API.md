---
title: "Trading 212 Public API"
source: "https://docs.trading212.com/api"
author:
published:
created: 2026-05-23
description:
tags:
  - "clippings"
---
## Trading 212 Public API (v0)

Welcome to the official API reference for the Trading 212 Public API! This guide provides all the information you need to start building your own trading applications and integrations.

---

## General Information

This API is currently in **beta** and is under active development. We're continuously adding new features and improvements, and we welcome your feedback.

### Only for Invest and Stocks ISA

The API described here is enabled and usable only for **Invest and Stocks ISA** account types.

### API Environments

We provide two distinct environments for development and trading:

- **Paper Trading (Demo):** `https://demo.trading212.com/api/v0`
- **Live Trading (Real Money):** `https://live.trading212.com/api/v0`

You can test your applications extensively in the paper trading environment without risking real funds before moving to live trading.

### ⚠️ API Limitations

Please be aware of the following limitations for any order placement:

- **Supported account types:** The Trading 212 Public API is enabled and usable only for **Invest and Stocks ISA** account types.
- **Order execution:** Orders can be executed only in the **primary account currency**
- **Multi-currency:** Multi-currency accounts are not currently supported through the API. Meaning your account, position and result values in the responses will be in the primary account currency.

### Key Concepts

- **Authentication:** Every request to the API must be authenticated using a secure key pair. See the **Authentication** section below for details.
- **Rate Limiting:** All API calls are subject to rate limits to ensure fair usage and stability. See the **Rate Limiting** section for a full explanation.
- **IP Restrictions:** For enhanced security, you can optionally restrict your API keys to a specific set of IP addresses from within your Trading 212 account settings.
- **Selling Orders:** To execute a sell order, you must provide a **negative** value for the `quantity` parameter (e.g., `-10.5`). This is a core convention of the API.

---

## Quickstart

This simple example shows you how to retrieve your account summary.

First, you must generate your API keys from within the Trading 212 app. For detailed instructions, please visit our Help Centre:

- [**How to get your Trading 212 API key**](https://helpcentre.trading212.com/hc/en-us/articles/14584770928157-Trading-212-API-key)

Once you have your **API Key** and **API Secret**, you can make your first call using `cURL`:

```
# Step 1: Replace with your actual credentials and Base64-encode them.

# The \`-n\` is important as it prevents adding a newline character.

CREDENTIALS=$(echo -n "<YOUR_API_KEY>:<YOUR_API_SECRET>" | base64)

# Step 2: Make the API call to the live environment using the encoded
credentials.

curl -X GET "https://live.trading212.com/api/v0/equity/account/summary" \
 -H "Authorization: Basic $CREDENTIALS"
```

---

## Authentication

The API uses a secure key pair for authentication on every request. You must provide your **API Key** as the username and your **API Secret** as the password, formatted as an HTTP Basic Authentication header.

The `Authorization` header is constructed by Base64-encoding your `API_KEY:API_SECRET` string and prepending it with `Basic `.

Here are examples of how to generate the required value in different environments.

**Linux or macOS (Terminal)**

You can use the `echo` and `base64` commands. Remember to use the `-n` flag with `echo` to prevent it from adding a trailing newline, which would invalidate the credential string.

```
# This command outputs the required Base64-encoded string for your header.

echo -n "<YOUR_API_KEY>:<YOUR_API_SECRET>" | base64
```

**Python**

This simple snippet shows how to generate the full header value.

```
import base64

# 1. Your credentials

api_key = "<YOUR_API_KEY>"

api_secret = "<YOUR_API_SECRET>"

# 2. Combine them into a single string

credentials_string = f"{api_key}:{api_secret}"

# 3. Encode the string to bytes, then Base64 encode it

encoded_credentials =
base64.b64encode(credentials_string.encode('utf-8')).decode('utf-8')

# 4. The final header value

auth_header = f"Basic {encoded_credentials}"

print(auth_header)
```

---

## Rate Limiting

To ensure high performance and fair access for all users, all API endpoints are subject to rate limiting.

> **IMPORTANT NOTE:** All rate limits are applied on a per-account basis, regardless of which API key is used or which IP address the request originates from.

Specific rate limits are detailed in the reference for each endpoint.

### How It Works

The rate limiter allows for requests to be made in bursts. For example, an endpoint with a limit of `50 requests per 1 minute` does **not** strictly mean you can only make one request every 1.2 seconds. Instead, you could:

- Make a burst of all 50 requests in the first 5 seconds of a minute. You would then need to wait for the reset time indicated by the `x-ratelimit-reset` header before making more requests.
- Pace your requests evenly, for example, by making one call every 1.2 seconds, ensuring you always stay within the limit.

### Function-Specific Limits

In addition to the general rate limits on HTTP calls, some actions have their own functional limits. For example, there is a maximum of **50 pending orders** allowed per ticker, per account.

## Pagination

All list endpoints in the API that return a collection of items (such as historical orders, dividends, and transactions) use **cursor-based pagination** to handle large data sets.

### Parameters

- **`limit`** (integer): Specifies the maximum number of items to return in a single request.
	- **Default:** 20
		- **Maximum:** 50
- **`cursor`** (string | number): A pointer to a specific item in the dataset. This tells the API where to start the next page of results.

### How to Paginate

The easiest way to paginate is by using the `nextPagePath` field returned in the response.

1. Make your initial request to a list endpoint (e.g., `/api/v0/equity/history/orders`) with an optional `limit` parameter. Do not include a `cursor`.
2. The API will return a response object. This object will contain a list of `items` and a `nextPagePath` field.
3. If the `nextPagePath` field is `null`, you have reached the end of the data, and there are no more pages.
4. If `nextPagePath` is not `null`, **use the entire string value of `nextPagePath`** as the path for your next request. This string contains all the necessary parameters (like `limit` and `cursor`) to get the next page.
5. Repeat this process until `nextPagePath` is `null`.

### Example

Here is a step-by-step example of fetching all transactions, 2 at a time.

**Request 1: Get the first page**

```
curl -X GET "https://demo.trading212.com/api/v0/equity/history/orders?limit=2" \
     -u "API_KEY:API_SECRET"
```

**Response 1: Note the nextPagePath**

```
{
  "items": [
    { "id": 987654321, "ticker": "AAPL_US_EQ", ... },
    { "id": 987654320, "ticker": "MSFT_US_EQ", ... }
  ],
  "nextPagePath": "/api/v0/equity/history/orders?limit=2&cursor=1760346100000"
}
```

**Request 2: Use the full nextPagePath for the next request**

```
curl -X GET "https://demo.trading212.com/api/v0/equity/history/orders?limit=2&cursor=1760346100000" \
     -u "API_KEY:API_SECRET"
```

**Response 2: Get the next page (and a new nextPagePath)**

```
{
  "items": [
    { "id": 987654319, "ticker": "AAPL_US_EQ", ... },
    { "id": 987654320, "987654318": "MSFT_US_EQ", ... }
  ],
  "nextPagePath": "/api/v0/equity/history/orders?limit=2&cursor=1660015723000"
}
```

**Request 3: Get the final page**

```
curl -X GET "https://demo.trading212.com/api/v0/equity/history/orders?limit=2&cursor=1660015723000" \
     -u "API_KEY:API_SECRET"
```

Response 3: nextPagePath is null, indicating the end

```
{
  "items": [
    { "id": 987654317, "ticker": "AMZN_US_EQ", ... }
  ],
  "nextPagePath": null
}
```

---

## Useful Links

Here are some additional resources that you may find helpful.

## Accounts

Access fundamental information about your trading account. Retrieve details such as your account ID, currency, and current cash balance.

Operations

## Instruments

Discover what you can trade. These endpoints provide comprehensive lists of all tradable instruments and the exchanges they belong to, including details like tickers and trading hours.

Operations

## Orders

**⚠️ Order Limitations**

- Orders can be executed only in the **main account currency**

Place, monitor, and cancel equity trade orders. This section provides the core functionality for programmatically executing your trading strategies for stocks and ETFs.

Operations

## Positions

Get a real-time overview of all your open positions, including quantity, average price, and current profit or loss.

Operations

## Historical events

Review your account's trading history. Access detailed records of past orders, dividend payments, and cash transactions, or generate downloadable CSV reports for analysis and record-keeping.

Operations

## Pies (Deprecated)

Manage your investment Pies. Use these endpoints to create, view, update, and delete your custom portfolios, making automated and diversified investing simple.

**Deprecation notice:** The current state of the Pies API, while still operational, won't be further supported and updated.

Operations

[[Personal Finance System]]