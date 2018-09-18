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
    "rrp": "159.99",
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

