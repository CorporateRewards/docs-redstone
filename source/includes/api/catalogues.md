# Catalogues

## List all Catalogues

This endpoint fetches a list of all catalogues available for your api key.

``` http
GET /api/v2/catalogues HTTP/1.1
Authorization: Token token=xxx
```

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

``` http
GET /api/v2/catalogues/1/products HTTP/1.1
Authorization: Token token=xxx
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": 357,
    "name": "LINKS OF LONDON 32-09-006 EFFERVESCENCE PENDANT",
    "base_price": 158.27,
    "description": "<p>A cluster of silver, white and grey pearls come together to create this elegant pendant complete with sterling silver chain. This pendant gives the traditional pearl a modern twist.</p>",
    "model_no": "LK-016",
    "sku": "LK-016",
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
        "sku": "LK-017",
        "available": true,
        "product_sku": "LK-016",
        "face_value": null,
        "product_id": null,
        "voucher_status": false,
        "variant_base_price": null,
        "face_value_gbp": null
      },
      {
        "id": 530,
        "variant": "large",
        "sku": "LK-018",
        "available": true,
        "product_sku": "LK-016",
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
        "url": "http://redstone-beta.s3.amazonaws.com/2014/10/21/13/38/44/960/lk_016.jpg"
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

`GET /api/v2/catalogues/1/products`

#### Attributes

Attribute | Type | Info
--------- | ---- | ----
status | string | ordering systems will only see products with a status of approved - this is largely functionless for ordering systems
minimum_age | integer | when present only products suitable for a minimum age of this value will be shown - for instance a value of 18 will only list products that have either no minimum age or a value greater then or equal to 18
delivery_type | string | String (may be comma separated) delivery_types are typically Physical, Email, Downloadable and Prepaid. Products will only have this or these delivery type if this parameter is used
country | string | This should be a proper case, url encoded string of a Country name (as reported in the Regions endpoint). Only products available in this coutry will be listed

### V3

Behaves the same as V2, but includes [translations](#translations) (where available).

``` http
GET /api/v3/catalogues/1/products HTTP/1.1
Authorization: Token token=xxx
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": 357,
    "name": "LINKS OF LONDON 32-09-006 EFFERVESCENCE PENDANT",
    "base_price": 158.27,
    "description": "<p>A cluster of silver, white and grey pearls come together to create this elegant pendant complete with sterling silver chain. This pendant gives the traditional pearl a modern twist.</p>",
    "model_no": "LK-016",
    "sku": "LK-016",
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
        "sku": "LK-017",
        "available": true,
        "product_sku": "LK-016",
        "face_value": null,
        "product_id": null,
        "voucher_status": false,
        "variant_base_price": null,
        "face_value_gbp": null
      },
      {
        "id": 530,
        "variant": "large",
        "sku": "LK-018",
        "available": true,
        "product_sku": "LK-016",
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
        "url": "http://redstone-beta.s3.amazonaws.com/2014/10/21/13/38/44/960/lk_016.jpg"
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

`GET /api/v3/catalogues/1/products`

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


``` http
GET /api/v4/catalogues/1/products HTTP/1.1
Authorization: Token token=xxx
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": 357,
    "name": "LINKS OF LONDON 32-09-006 EFFERVESCENCE PENDANT",
    "base_price": 158.27,
    "brand_id": 1,
    "max_value": 38.95,
    "description": "<p>A cluster of silver, white and grey pearls come together to create this elegant pendant complete with sterling silver chain. This pendant gives the traditional pearl a modern twist.</p>",
    "model_no": "LK-016",
    "sku": "LK-016",
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
        "sku": "LK-017",
        "available": true,
        "product_sku": "LK-016",
        "face_value": null,
        "product_id": null,
        "voucher_status": false,
        "variant_base_price": null,
        "face_value_gbp": null
      },
      {
        "id": 530,
        "variant": "large",
        "sku": "LK-018",
        "available": true,
        "product_sku": "LK-016",
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
        "url": "http://redstone-beta.s3.amazonaws.com/2014/10/21/13/38/44/960/lk_016.jpg"
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

`GET /api/v4/catalogues/1/products`

#### Parameters

Parameter | Type | Info
--------- | ---- | ----
id | Array | Otptional - A list of product ids. This will return products which have ids that match an integer supplied in this list.
status | String | Optional - Only products with this status will be returned. For example 'Pending' or 'Approved'.
sku | String | Optional - Specific product SKU.
country_id | Array | Otptional - A list of country ids. This will return products which have associated countries with an id that matches an integer supplied in this list.
availability | String | Optional - Indicates whether a product is available, the value must be either 'True' or 'False'.
brand_id | Integer | Optional - Only products linked to this brand_id will be returned.
page | Integer | Optional - The requested page number. Defaults to page 1 if not supplied. Optional.