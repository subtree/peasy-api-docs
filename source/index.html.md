---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript


toc_footers:
  - <a href='mailto:support@peasy.nu?subject=I need an API Key'>Contact us for a Developer Key</a>
  - <a href="https://github.com/subtree/peasy-api-docs">Found an error? Help us improve!</a>

includes:
  - errors

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

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://v3.api.peasy.nu/api/whoami" \
  -H "Authorization: YOUR_SECRET_API_KEY"
```

```javascript
const axios = require('axios');

const url = 'https://v3.api.peasy.nu/api/whoami'
const response = await axios.get(url, 
  { headers: { 'Authorization': 'YOUR_SECRET_API_KEY' } })
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

Peasy.nu expects the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: YOUR_SECRET_API_KEY`

You can request a Peasy.nu API key by [contacting us](mailto:support@peasy.nu).

<aside class="notice">
You must replace <code>YOUR_SECRET_API_KEY</code> with your personal API key.
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

## Who Am I?

```shell
curl "https://v3.api.peasy.nu/api/whoami" \
  -H "Authorization: YOUR_SECRET_API_KEY"
```

```javascript
const axios = require('axios');

const url = 'https://v3.api.peasy.nu/api/whoami'
const response = await axios.get(url, 
  { headers: { 'Authorization': 'YOUR_SECRET_API_KEY' } })
console.log(response.data)
```

> The above command returns JSON structured like this:

```json
{"message":"Hello Test Company!"}
```

Secured endpoint to make sure your <code>API_KEY</code> is valid and that you are correctly identified as a user.

### HTTP Request

`GET https://v3.api.peasy.nu/api/whoami`


# Reading statistics


## Get stats

```shell
curl "https://v3.api.peasy.nu/api/stats?startDate=2021-07-30&period=DAY&format=json&currency=EUR" \
  -H "Authorization: YOUR_SECRET_API_KEY"
```

```javascript
const axios = require('axios');

const url = 'https://v3.api.peasy.nu/api/stats?startDate=2021-07-30&period=DAY&format=json&currency=EUR'
const response = await axios.get(url, 
  { headers: { 'Authorization': 'YOUR_SECRET_API_KEY' } })
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

<aside class="warning">All stats is currently stored internally in SEK, and when you select another currency, we use the ECB exchange rate for the <code>startDate</code>. If the rate has shifted meaningfully during a week or month, this may lead to unexpected values.</aside>

### HTTP Request

`GET https://v3.api.peasy.nu/api/stats`

### URL Parameters

Parameter | Description
--------- | -----------
startDate | The first date of the period to fetch stats for. When requesting for a period, make sure the given date is the first date (a Monday for a <code>WEEK</code> or the first in the month for a <code>MONTH</code>) in a completed period. Default is yesterday.
period | What period to fetch stats for. Must be either `DAY`, `WEEK` or `MONTH`. Default is `DAY`.
format | The format of the results. Must be either `json` or `csv`. Default is `json`.
currency | The currency to display all monetary values in. Uses the official ECB exchange rate for the order date. Default is `SEK`.


# Reading order data


## Get orders

```shell
curl "https://v3.api.peasy.nu/api/orders?startDate=2021-07-30&endDate=2021-07-30&format=json&currency=EUR" \
  -H "Authorization: YOUR_SECRET_API_KEY"
```

```javascript
const axios = require('axios');

const url = 'https://v3.api.peasy.nu/api/orders?startDate=2021-07-30&endDate=2021-07-30&format=json&currency=EUR'
const response = await axios.get(url, 
  { headers: { 'Authorization': 'YOUR_SECRET_API_KEY' } })
console.log(response.data)
```

> The above command returns JSON structured like this:

```json
[
  {"orderDateTime":"2021-06-01T09:46:36.000+02:00","orderId":"6229","orderItemsCount":1,"totalCostExcludingVAT":0},
  {"orderDateTime":"2021-06-01T15:50:16.000+02:00","orderId":"7863","orderItemsCount":1,"totalCostExcludingVAT":61240.08},
  {"orderDateTime":"2021-06-01T10:59:55.000+02:00","orderId":"8245","orderItemsCount":2,"totalCostExcludingVAT":182565.2},
  {"orderDateTime":"2021-06-01T09:10:00.000+02:00","orderId":"8293","orderItemsCount":1,"totalCostExcludingVAT":82443.1}
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
currency | The currency to display all monetary values in. Uses the official ECB exchange rate for the order date. Default is `SEK`.

# Changelog

### 2022-01-13
* Initial release


<small>Copyright 2022 Peasy.nu</small>