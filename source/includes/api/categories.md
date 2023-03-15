# Categories

## List categories

### V2

This endpoint fetches a JSON array of categories. There is a simple parent relationship represented using the `parent_id` field. This enables an ordering system that chooses to use the Categories from GPS to re-create the category structure. Product data has a reference to the GPS Category they are in using the `category_id` field.

> Request

``` http
GET /api/v2/categories HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id" : 1,
    "name" : "category name",
    "parent_id" : null
  },
  {
    "id" : 2,
    "name" : "category name",
    "parent_id" : 1
  }
  ...
]
```

#### HTTP Request

`GET /api/v2/categories`

### V3

Behaves the same as V2, but includes [translations](#translations) (where available).

> Request

``` http
GET /api/v3/categories HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id" : 1,
    "name" : "category name",
    "parent_id" : null,
    "translations": [
      {
        "locale": "fr",
        "name": "category name in French"
      },
      {
        "locale": "es",
        "name": "category name in Spanish"
      }
    ]
  },
  {
    "id" : 2,
    "name" : "category name",
    "parent_id" : 1,
    "translations": []
  }
  ...
]
```

#### HTTP Request

`GET /api/v3/categories`
