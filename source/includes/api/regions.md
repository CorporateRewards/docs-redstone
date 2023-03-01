# Regions

## List all Regions and the Countries within them

This endpoint fetches a JSON array of Regions each with an array of countries in that region. Each region will show a list of the countries in that region as a nested list of objects.

> Request

``` http
GET /api/v2/regions HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id" : 1,
    "name" : "Western Europe",
    "countries" : [
      {
        "id" : 1,
        "name" : "United Kingdom",
        "currency_id" : 2,
        "vat_rate_id" : 6,
        "iso_code" : "UK",
        "dialing_code": "+44",
        "currency": {
            "id": 1,
            "title": "Pounds Sterling",
            "abbreviation": "GBP",
            "sign": "£",
            "name": "Pounds Sterling",
            "exchange_rate": 1
        }
      },
      {
        "id" : 2,
        "name" : "France",
        "currency_id" : 6,
        "vat_rate_id" : 3,
        "iso_code" : "FR",
        ,
        "currency": {
            "id": 2,
            "title": "Euro",
            "abbreviation": "EUR",
            "sign": "€",
            "name": "Euro",
            "exchange_rate": 1.23
        }
      }
    ]
  },
  ...
]
```

### HTTP Request

`GET /api/v2/regions`
