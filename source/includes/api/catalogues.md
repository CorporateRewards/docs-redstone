# Catalogues

## List all Catalogues

This endpoint fetches a list of all catalogues available for your api key.

> Request

``` http
GET /api/v2/catalogues HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id" : 1,
    "name" : "Core",
    "exclusive": false,
    "created_at": "2014-10-27T12:22:26.000Z",
    "updated_at": "2014-10-27T12:22:26.000Z"
  },
  {
    "id" : 2,
    "name" : "Vouchers",
    "exclusive": false,
    "created_at": "2014-10-27T12:22:26.000Z",
    "updated_at": "2014-10-27T12:22:26.000Z"
  },
  ...
]
```

#### HTTP Request

`GET /api/v2/catalogues`

#### Attributes

Attribute | Type | Info
--------- | ---- | ----
id | integer | primary key for catalogues

## List all products in a catalogue

### V2

This is how ordering systems will access the products available to them. Typically, after fetching a list of catalogues, an ordering system will iterate over each catalogue, fetching the products in each one to add to a local store/cache of products.

> Request

``` http
GET /api/v2/catalogues/{id}/products HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": 357,
    "name": "Product Name",
    "base_price": 158.27,
    "description": "Product Description",
    "model_no": "A-123",
    "sku": "A-123",
    "available": true,
    "availability_note": "",
    "international_requirements": false,
    "minimum_age": null,
    "primary_image_id": 337,
    "variant": "",
    "voucher": null,
    "lowest_denomination": null,
    "face_value": null,
    "supplier_currency_id": 31,
    "available_countries": [
      {
        "id": 140,
        "name": "UK",
        "vat_rate": "20%",
        "delivery_charge": 7.2
      },
      {
        "id": 165,
        "name": "Albania",
        "vat_rate": "20%",
        "delivery_charge": 34.75
      },
    ...
    ],
    "rrp": "159.99",
    "supplier": {
      "id": 14
    },
    "vat_rate": {
      "name": "20%",
      "numeric": 20
    },
    "status": {
      "name": "Approved"
    },
    "catalogue": {
      "id": 8,
      "name": "Demo Catalogue",
      "exclusive": false
    },
    "categories": [
      {
        "id": 62,
        "name": "Necklaces",
        "parent_id": 59,
        "ar_association_key_name": 414
      }
    ],
    "variants": [
      {
        "id": 529,
        "variant": "small",
        "sku": "B-123",
        "available": true,
        "product_sku": "A-123",
        "face_value": null,
        "product_id": null,
        "voucher_status": false,
        "variant_base_price": null,
        "face_value_gbp": null
      },
      {
        "id": 530,
        "variant": "large",
        "sku": "B-234",
        "available": true,
        "product_sku": "A-123",
        "face_value": null,
        "product_id": null,
        "voucher_status": false,
        "variant_base_price": null,
        "face_value_gbp": null
      }
    ],
    "media": [
      {
        "id": 337,
        "url": "https://images.com/123533.jpg"
      }
    ],
    "delivery_type": {
      "id": 1,
      "name": "Physical",
      "requires_address": true
    },
    "currency": {
      "title": "Pounds Sterling",
      "abbreviation": "GBP",
      "sign": "£"
    }
  },
  ...
]
```

#### HTTP Request

`GET /api/v2/catalogues/{id}/products`

#### Attributes

Attribute | Type | Info
--------- | ---- | ----
status | string | ordering systems will only see products with a status of approved - this is largely functionless for ordering systems
minimum_age | integer | when present only products suitable for a minimum age of this value will be shown - for instance a value of 18 will only list products that have either no minimum age or a value greater then or equal to 18
delivery_type | string | String (may be comma separated) delivery_types are typically Physical, Email, Downloadable and Prepaid. Products will only have this or these delivery type if this parameter is used
country | string | This should be a proper case, url encoded string of a Country name (as reported in the Regions endpoint). Only products available in this coutry will be listed

### V3

Behaves the same as V2, but includes [translations](#translations) (where available).

> Request

``` http
GET /api/v3/catalogues/{id}/products HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": 357,
    "name": "Product Name",
    "base_price": 158.27,
    "description": "Product Description",
    "model_no": "A-123",
    "sku": "A-123",
    "available": true,
    "availability_note": "",
    "international_requirements": false,
    "minimum_age": null,
    "primary_image_id": 337,
    "variant": "",
    "voucher": null,
    "lowest_denomination": null,
    "face_value": null,
    "supplier_currency_id": 31,
    "available_countries": [
      {
        "id": 140,
        "name": "UK",
        "vat_rate": "20%",
        "delivery_charge": 7.2
      },
      {
        "id": 165,
        "name": "Albania",
        "vat_rate": "20%",
        "delivery_charge": 34.75
      },
    ...
    ],
    "rrp": "159.99",
    "supplier": {
      "id": 14
    },
    "vat_rate": {
      "name": "20%",
      "numeric": 20
    },
    "status": {
      "name": "Approved"
    },
    "catalogue": {
      "id": 8,
      "name": "Demo Catalogue",
      "exclusive": false
    },
    "categories": [
      {
        "id": 62,
        "name": "Necklaces",
        "parent_id": 59,
        "ar_association_key_name": 414
      }
    ],
    "variants": [
      {
        "id": 529,
        "variant": "small",
        "translations": [
          {
            "locale": "fr",
            "name": "petit"
          },
          {
            "locale": "de",
            "name": "klein"
          }
        ],
        "sku": "B-123",
        "available": true,
        "product_sku": "A-123",
        "face_value": null,
        "product_id": null,
        "voucher_status": false,
        "variant_base_price": null,
        "face_value_gbp": null
      },
      {
        "id": 530,
        "variant": "large",
        "sku": "B-234",
        "available": true,
        "product_sku": "A-123",
        "face_value": null,
        "product_id": null,
        "voucher_status": false,
        "variant_base_price": null,
        "face_value_gbp": null
      }
    ],
    "media": [
      {
        "id": 337,
        "url": "https://images.com/123533.jpg"
      }
    ],
    "delivery_type": {
      "id": 1,
      "name": "Physical",
      "requires_address": true
    },
    "currency": {
      "title": "Pounds Sterling",
      "abbreviation": "GBP",
      "sign": "£"
    },
    "translations": [
      {
        "locale": "fr",
        "name": "product_name_french",
        "description": "product description_french"
      },
      {
        "locale": "de",
        "name": "product_name_german",
        "description": "product description_german"
      }
    ]
  },
  ...
]
```

#### HTTP Request

`GET /api/v3/catalogues/{id}/products`

#### Attributes

Attribute | Type | Info
--------- | ---- | ----
status | string | ordering systems will only see products with a status of approved - this is largely functionless for ordering systems
minimum_age | integer | when present only products suitable for a minimum age of this value will be shown - for instance a value of 18 will only list products that have either no minimum age or a value greater then or equal to 18
delivery_type | string | String (may be comma separated) delivery_types are typically Physical, Email, Downloadable and Prepaid. Products will only have this or these delivery type if this parameter is used
country | string | This should be a proper case, url encoded string of a Country name (as reported in the Regions endpoint). Only products available in this coutry will be listed

### V4

This endpoint is available for ordering systems that wish to access the list of products available to them. It behaves in a similar way to the V3 endpoint, but includes pagination, query parameters and additional fields such as `brand_id` and `max_value` (please note that all price values are shown in GBP).

<aside class="warning">Note that this endpoint is currently <strong>only</strong> to be used by ordering systems, <strong>not</strong>  supplier systems.</aside>

> Request

``` http
GET /api/v4/catalogues/{id}/products HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": 357,
    "name": "Product Name",
    "base_price": 158.27,
    "description": "Product Description",
    "model_no": "A-123",
    "sku": "A-123",
    "available": true,
    "availability_note": "",
    "international_requirements": false,
    "minimum_age": null,
    "primary_image_id": 337,
    "variant": "",
    "voucher": false,
    "lowest_denomination": null,
    "face_value": null,
    "supplier_currency_id": 31,
    "available_countries": [
      {
        "id": 140,
        "name": "UK",
        "vat_rate": "20%",
        "delivery_charge": 7.2
      },
      {
        "id": 165,
        "name": "Albania",
        "vat_rate": "20%",
        "delivery_charge": 34.75
      },
    ...
    ],
    "brand_id": 1,
    "max_value": 200,
    "rrp": "159.99",
    "supplier": {
      "id": 14
    },
    "vat_rate": {
      "name": "20%",
      "numeric": 20
    },
    "status": {
      "name": "Approved"
    },
    "catalogue": {
      "id": 8,
      "name": "Demo Catalogue",
      "exclusive": false
    },
    "categories": [
      {
        "id": 62,
        "name": "Necklaces",
        "parent_id": 59
      }
    ],
    "variants": [
      {
        "id": 63489,
        "variant": "Small",
        "sku": "B-123",
        "translations": [
            {
                "locale": "fr",
                "variant": "Petit"
            }
        ],
        "available": true,
        "product_sku": "A-123",
        "face_value": null,
        "product_id": null,
        "voucher_status": false,
        "variant_base_price": 354.6728,
        "face_value_gbp": null
      },
      {
        "id": 63490,
        "variant": "Large",
        "sku": "B-234",
        "translations": [
          {
            "locale": "fr",
            "variant": "Grande"
          }
        ],
        "available": true,
        "product_sku": "A-123",
        "face_value": null,
        "product_id": null,
        "voucher_status": false,
        "variant_base_price": null,
        "face_value_gbp": null
      }
    ],
    "media": [
      {
        "id": 337,
        "url": "https://images.com/123533.jpg"
      }
    ],
    "delivery_type": {
      "id": 1,
      "name": "Physical",
      "requires_address": true
    },
    "currency": {
      "title": "Pounds Sterling",
      "abbreviation": "GBP",
      "sign": "£"
    },
    "translations": [
      {
        "locale": "fr",
        "name": "product_name_french",
        "description": "product description_french"
      },
      {
        "locale": "de",
        "name": "product_name_german",
        "description": "product description_german"
      }
    ]
  },
  ...
]
```

#### HTTP Request

`GET /api/v4/catalogues/{id}/products`

#### Parameters

Parameter | Type | Info
--------- | ---- | ----
id | Array | Optional - A list of product ids. This will return products which have ids that match an integer supplied in this list.
status | String | Optional - Only products with this status will be returned. For example 'Pending' or 'Approved'.
sku | String | Optional - Specific product SKU.
country_id | Array | Optional - A list of country ids. This will return products which have associated countries with an id that matches an integer supplied in this list.
availability | String | Optional - Indicates whether a product is available, the value must be either 'True' or 'False'.
brand_id | Integer | Optional - Only products linked to this brand_id will be returned.
page | Integer | Optional - The requested page number. Defaults to page 1 if not supplied. Optional.