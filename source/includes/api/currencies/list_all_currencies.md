## List of all currencies

### Get all currencies

An endpoint to retrieve all currencies and their current exchange rate.

``` http
GET /api/v2/currencies HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1,
  "title": "Great British Pound",
  "abbreviation": "GBP",
  "sign": "£",
  "name": "Great British Pound £",
  "created_at": "01/01/2000 00:00:00",
  "updated_at": "01/02/2000 00:00:00",
  "exchange_rate": 1.1111
}
```

#### HTTP Request

`GET /api/v2/currencies`

#### Body Parameters

Attribute | Type | Info
--------- | ---- | ----
id | integer | The id of the currency
title | string| The name of the currency
abbreviation| string| 3 letter abbreviation
sign| string| UTF8 character to represent currency
name | string | Concatenation of the title and sign
created_at | datetime | Datetime in SQL format indicating when this currency was created
updated_at | datetime | Datetime in SQL format indicating when this currency was last updated
exchange_rate| decimal | The currencies current exchange rate (to 4 decimal places)
