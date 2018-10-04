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
