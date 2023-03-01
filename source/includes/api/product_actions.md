# Product actions

Available on v3 of the api only

## Approve product

Changes the status of a product to approved. A product must have at least one country, one catalogue, one category, and one image before it can be approved.

> Request

``` http
POST /api/v3/products/:id/approve HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```

#### HTTP Request

`POST /api/v3/products/:id/approve`

#### URL Parameters

Attribute | Type | Info
--------- | ---- | ----
id | integer | The id of the product


## Put product on hold

Changes the status of a product to on hold

> Request

``` http
POST /api/v3/products/:id/onhold HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```

#### HTTP Request

`POST /api/v3/products/:id/onhold`

#### URL Parameters

Attribute | Type | Info
--------- | ---- | ----
id | integer | The id of the product

## Archive product

Changes the status of a product to archived

> Request

``` http
POST /api/v3/products/:id/archive HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```

#### HTTP Request

`POST /api/v3/products/:id/archive`

#### URL Parameters

Attribute | Type | Info
--------- | ---- | ----
id | integer | The id of the product
