### List all currencies

An endpoint to retrieve all currencies and their current exchange rate.
Your API key must have permission to read currencies.

> Request

``` http
GET /api/v2/currencies HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1,
  "title": "Great British Pound",
  "abbreviation": "GBP",
  "sign": "£",
  "name": "Great British Pound (£)",
  "created_at": "01/01/2000 00:00:00",
  "updated_at": "01/02/2000 00:00:00",
  "exchange_rate": 1.1111
}
```

#### HTTP Request

`GET /api/v2/currencies`

### Update exchange rates

An endpoint to update the exchange rate of a specified currency. Your API key must have permission to write currencies.

> Request

``` http
PATCH /api/v2/currencies/{id} HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
  "exchange_rate": 1.1111
}
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1,
  "name": "Great British Pound (£)",
  "exchange_rate": 1.1111
}
```

#### HTTP Request

`PATCH /api/v2/currencies/{id}`

#### URL Parameters

Attribute | Type | Info
--------- | ---- | ----
id | integer | The id of the currency

#### Body Parameters

Attribute | Type | Info
--------- | ---- | ----
exchange_rate| decimal | The currencies current exchange rate (to 4 decimal places)
