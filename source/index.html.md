---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript


toc_footers:
  - <a href='mailto:support@peasy.nu?subject=I need an API Key'>Contact us for a Developer Key</a>
  - <a href="https://github.com/subtree/peasy-api-docs">Found an error? Help us improve!</a>

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Peasy.nu API
---

# Introduction

Welcome to the <a href="https://www.peasy.nu">Peasy.nu</a> API! You can use our API to read order data, analytics and more. 

We currently offer a REST API accessible using any standards-based client and have provided examples using `curl` and Javascript. Let us know if you need help for your use-case! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

If you ever have questions, don't hesistate to [contact us](mailto:support@peasy.nu).

# Authentication

> To authorize, either supply your secret API_KEY in an Authorization header or as a query parameter. 

```shell
# With shell, you can just pass the correct header with each request
curl "https://v3.api.peasy.nu/api/whoami" \
  -H "Authorization: apikey YOUR_SECRET_API_KEY"

# Alternatively, you can use supply the API_KEY_ as a query parameter, but this is 
# not recommended as the API_KEY is more likely to be exposed to the public due to 
# it being shown in history files etc.
curl "https://v3.api.peasy.nu/api/whoami?apikey=YOUR_SECRET_API_KEY"
```

```javascript
const axios = require('axios');

// Recommended way to pass the API_KEY is using a header
const url = 'https://v3.api.peasy.nu/api/whoami'
const response = await axios.get(url, 
  { headers: { Authorization': 'apikey ' + 'YOUR_SECRET_API_KEY' } })
console.log(response.data)

// Alternatively, you can use the query parameter
const url = 'https://v3.api.peasy.nu/api/whoami?apikey=YOUR_SECRET_API_KEY'
const response = await axios.get(url)
console.log(response.data)
```

> If the supplied `API_KEY` is correct, you will get a reply like this:

```json
{"message":"Hello Test Company!"}
```


> If the `API_KEY` is incorrect, you will instead get an error:

```json
{"message":"Forbidden"}
```

> Make sure to replace `YOUR_SECRET_API_KEY` with your API key. 

Peasy.nu expects the API key to be included in all API requests to the server either in a query parameter or in a header that looks like the following:

`Authorization: apikey YOUR_SECRET_API_KEY`

You can request a Peasy.nu API key by [contacting us](mailto:support@peasy.nu).

<aside class="notice">
In all instances, you must replace <code>YOUR_SECRET_API_KEY</code> with your own, personal API key.
</aside>

# Testing your connection

## Ping

```shell
curl "https://v3.api.peasy.nu/api/ping" 
```

```javascript
const axios = require('axios');

const url = 'https://v3.api.peasy.nu/api/ping'
const response = await axios.get(url)
console.log(response.data)
```

> The above command returns JSON structured like this:

```json
{"message": "Hello stranger!"}
```

This endpoint is the only one that doesn't require authentication, and is provided as a convinience to make sure you can access the API at all. 

### HTTP Request

`GET https://v3.api.peasy.nu/api/ping`


### URL Parameters

Parameter | Description
--------- | -----------
format | The format of the results. Must be either `json` or `csv`. Default is `json`.


## Who Am I?

```shell
curl "https://v3.api.peasy.nu/api/whoami" \
  -H "Authorization: apikey YOUR_SECRET_API_KEY"
```

```javascript
const axios = require('axios');

const url = 'https://v3.api.peasy.nu/api/whoami'
const response = await axios.get(url, 
  { headers: { Authorization': 'apikey ' + 'YOUR_SECRET_API_KEY' } })
console.log(response.data)
```

> The above command returns JSON structured like this:

```json
{"message":"Hello Test Company!"}
```

Secured endpoint to make sure your <code>API_KEY</code> is valid and that you are correctly identified as a user.

### HTTP Request

`GET https://v3.api.peasy.nu/api/whoami`


### URL Parameters

Parameter | Description
--------- | -----------
format | The format of the results. Must be either `json` or `csv`. Default is `json`.


# Reading statistics


## Get stats

```shell
curl "https://v3.api.peasy.nu/api/stats?startDate=2021-07-30&period=DAY&format=json&currency=EUR" \
  -H "Authorization: apikey YOUR_SECRET_API_KEY"
```

```javascript
const axios = require('axios');

const url = 'https://v3.api.peasy.nu/api/stats?startDate=2021-07-30&period=DAY&format=json&currency=EUR'
const response = await axios.get(url, 
  { headers: { Authorization': 'apikey ' + 'YOUR_SECRET_API_KEY' } })
console.log(response.data)
```

> The above command returns JSON structured like this:

```json
{
  "averageOrderValue": 1236.04,
  "cancelledOrderCount": 0,
  "conversionRatio": 0.06,
  "currency": "EUR",
  "exchangeRate": 0.09816625436839832,
  "freeOrderCount": 7,
  "itemsPerOrder": 1.36,
  "orderCount": 109,
  "orderItemsCount": 148,
  "ordersWithVouchers": 25,
  "payloadVersion": "1.0.0",
  "period": "DAY",
  "ratioOfOrdersWithVouchers": 0.23,
  "revenue": 134728.53,
  "sessionsCount": 1884,
  "startDate": "2021-07-30",
  "updatedAt": "2021-08-23T05:08:53.505+02:00",
  "voucherCount": 25
}
```

This commands retrieves statistics for a given period, similar to what the stats email delivers. All monetary values are expressed as cents, meaning <code>1 EUR</code> would be represented as <code>100</code>.

<aside class="warning">All stats is currently stored internally in <code>SEK</code>, and when you select another currency, we use the ECB exchange rate for the <code>startDate</code>. If you read a <code>WEEK</code> or <code>MONTH</code> period and the rate has shifted meaningfully during the period, this may lead to unexpected values.</aside>

### HTTP Request

`GET https://v3.api.peasy.nu/api/stats`

### URL Parameters

Parameter | Description
--------- | -----------
startDate | The first date of the period to fetch stats for. When requesting for a period, make sure the given date is the first date (a Monday for a `WEEK` or the first in the month for a `MONTH`) in a completed period. Default is yesterday.
period | What period to fetch stats for. Must be either `DAY`, `WEEK` or `MONTH`. Default is `DAY`.
format | The format of the results. Must be either `json` or `csv`. Default is `json`.
currency | The currency to display all monetary values in. Uses the official ECB exchange rate for the order date. Default is `SEK`.
decimalSeparator | The decimal separator to use when formatting floating point values. Default is `.`. Only used when `format` is `csv`.


# Reading order data


## Get orders

```shell
curl "https://v3.api.peasy.nu/api/orders?startDate=2021-07-30&endDate=2021-07-30&format=json&currency=EUR&includeItems=true" \
  -H "Authorization: apikey YOUR_SECRET_API_KEY"
```

```javascript
const axios = require('axios');

const url = 'https://v3.api.peasy.nu/api/orders?startDate=2021-07-30&endDate=2021-07-30&format=json&currency=EUR&includeItems=true'
const response = await axios.get(url, 
  { headers: { Authorization': 'apikey ' + 'YOUR_SECRET_API_KEY' } })
console.log(response.data)
```

> The above command returns JSON structured like this:

```json
[
  {
    "orderDateTime": "2021-06-01T15:50:16.000+02:00",
    "orderId": "7863",
    "orderItemsCount": 1,
    "totalCostExcludingVAT": 61240,
    "totalValueExcludingVAT": 61240,
    "totalDiscountExcludingVAT": 0,
    "freightCostExcludingVAT": 0,
    "paymentCostExcludingVAT": 0,
    "items": [
      {
        "id": "FOOBAR",
        "name": "Green shirt",
        "quantity": 1,
        "totalPriceExcludingVAT": 61240,
        "listPriceExcludingVAT": 61240,
        "discountExcludingVAT": 0
      }
    ]
  },
  {
    "orderDateTime": "2021-06-01T10:59:55.000+02:00",
    "orderId": "8245",
    "orderItemsCount": 2,
    "totalCostExcludingVAT": 172565,
    "totalValueExcludingVAT": 182565,
    "totalDiscountExcludingVAT": 10000,
    "freightCostExcludingVAT": 0,
    "paymentCostExcludingVAT": 0,
    "items": [
      {
        "id": "58008",
        "name": "Green shirt",
        "quantity": 1,
        "totalPriceExcludingVAT": 61240,
        "listPriceExcludingVAT": 71240,
        "discountExcludingVAT": 10000
      },
      {
        "id": "3735928559",
        "name": "Black Pants",
        "quantity": 1,
        "totalPriceExcludingVAT": 121325,
        "listPriceExcludingVAT": 121325,
        "discountExcludingVAT": 0
      }
    ]
  },
  {
    "orderDateTime": "2021-06-01T09:10:00.000+02:00",
    "orderId": "8293",
    "orderItemsCount": 1,
    "totalCostExcludingVAT": 82443,
    "totalValueExcludingVAT": 82443,
    "totalDiscountExcludingVAT": 0,
    "freightCostExcludingVAT": 0,
    "paymentCostExcludingVAT": 0,
    "items": [
      {
        "id": "1337",
        "name": "Windbreaker",
        "quantity": 1,
        "totalPriceExcludingVAT": 82443,
        "listPriceExcludingVAT": 82443,
        "discountExcludingVAT": 0
      }
    ]
  }
]
```

This commands retrieves some data about all orders for the date period specified. All monetary values are expressed as cents, meaning <code>1 EUR</code> would be represented as <code>100</code>.

### HTTP Request

`GET https://v3.api.peasy.nu/api/orders`

### URL Parameters

Parameter | Description
--------- | -----------
startDate | The first date to include in the results. Default is today.
endDate | The last date to include in the results. Default is today.
format | The format of the results. Must be either `json` or `csv`. Default is `json`.
includeItems | Return data on items for all orders. Must be either `true` or `false`. Default is `false`. If set to `true` and format is `csv`, the response will include 1 row per item (instead of the default 1 row per order), thereby possibly duplicating common order data.
currency | The currency to display all monetary values in. Uses the official ECB exchange rate for the order date. Default is `SEK`.
decimalSeparator | The decimal separator to use when formatting floating point values. Default is `.`. Only used when `format` is `csv`.
fetchCancelledOrders | Fetch orders with all possible statuses, not only `PROCESSING` and `COMPLETED`. Default is `false`.

# Errors

The API uses the following error codes:


Error&nbsp;Code | Meaning
---------- | -------
401 | Unauthorized -- Your API key is wrong or missing.
404 | Not Found -- The requested object could not be found.
422 | Unprocessable Entity -- Your request is invalid in some way, see the <code>message</code> attribute for a clue.
500 | Internal Server Error -- We have a problem with our server. Try again later and if the problem persists, contact us.


# Changelog

### 2022-03-17
* Added more data on `GET /api/orders`:
  * `orderStatus`
  * `totalValueExcludingVAT`
  * `totalDiscountExcludingVAT`
  * `freightCostExcludingVAT`
  * `paymentCostExcludingVAT`
  * `item[].listPriceExcludingVAT`
  * `item[].discountExcludingVAT`
* Now possible to list orders with all possible `orderStatus`. Add `fetchCancelledOrders=true` to the URL parameters for `GET /api/orders`.

### 2022-03-10
* Added `useCommaSeparator` parameter to `GET /api/stats` and `GET /api/orders` that enables the use of a comma as decimal separator. Useful for European users. Default is `false`.

### 2022-01-18
* Authorization header now expects type of `apikey` as prefix before the actual `API_KEY`
* Added support for specifying the `API_KEY` as a query parameter
* Remove fractions from monetary values (since they are all in cents) 
* Add possibility to retrieve item data in order items as part of call to `/orders` by specifiying `includeItems=true` as a query parameter
* Added support for the `format` query parameter for all endpoints


### 2022-01-13
* Initial release


<small>Copyright 2022 Peasy.nu</small>