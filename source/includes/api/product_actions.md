# Product actions

Available on v3 of the API only

## Approve product

Changes the status of a product to approved. A product must have at least one country, one catalogue, one category, and one image before it can be approved.

> Request

``` http
POST /api/v3/products/{id}/approve HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```

#### HTTP Request

`POST /api/v3/products/{id}/approve`

#### URL Parameters

Attribute | Type | Info
--------- | ---- | ----
id | integer | The id of the product


## Put product on hold

Changes the status of a product to on hold

> Request

``` http
POST /api/v3/products/{id}/onhold HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```

#### HTTP Request

`POST /api/v3/products/{id}/onhold`

#### URL Parameters

Attribute | Type | Info
--------- | ---- | ----
id | integer | The id of the product

## Archive product

Changes the status of a product to archived

> Request

``` http
POST /api/v3/products/{id}/archive HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```

#### HTTP Request

`POST /api/v3/products/{id}/archive`

#### URL Parameters

Attribute | Type | Info
--------- | ---- | ----
id | integer | The id of the product

## Change availability

Changes the availability of a product based on SKU, and therefore can be used to change availability of products or variants/denominations. This API can only be used by suppliers.

#### HTTP Request

`POST /api/v3/products/availability`

#### Request Parameters

Attribute | Type | Info
--------- | ---- | ----
sku | string | Required - The sku of the item
available  | boolean  |  Required - true or false
availability_note  | string  |  Conditional - required if available is set to false. This text will be displayed in GPS only and informs our team why the product is not available such as "Coming soon" or "Temporarily out of stock"

> Request

``` http
POST /api/v3/products/availability HTTP/1.1
Authorization: Token token=xxx

{ 
    "sku": "product123",
    "available": false,
    "availability_note": "Out of stock until dd/mm/yyyy"
}
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```
