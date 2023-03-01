## List all products

This endpoint shows all products associated with this api key as a list

> Request

``` http
GET /api/v2/products HTTP/1.1
Authorization: Token token=xxx

```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/JSON

[
  {
    "id": 22413,
    "base_price": 100.0,
    "model_no": "bub001",
    "sku": "000123454",
    "available": true,
    "availability_note": null,
    "international_requirements": false,
    "minimum_age": 0,
    "primary_image_id": 6419,
    "variant": null,
    "voucher": false,
    "lowest_denomination": null,
    "face_value":null,
    "supplier_currency_id": 1,
    "brand_id": null,
    "name": "Bat Utility Belt",
    "description": "<p>Quite simply the quintessential superhero utility belt. Iconnic and functional - no bat-based superhero should be without it </p>",
    "available_countries": [
        {
            "id": 1,
            "name": "USA",
            "vat_rate": "0%",
            "delivery_charge": 0.0
        }
    ],
    "rrp": "100",
    "supplier": {
        "id": 66
    },
    "vat_rate": {
        "name": "0%",
        "numeric": 0.0
    },
    "status": {
        "name": "Approved"
    },
    "catalogue": {
        "id": 1,
        "name": "Superhero Equipment",
        "exclusive": false
    },
    "categories": [
        {
            "id": 407,
            "parent_id": 75,
            "name": "Outerwear"
        }
    ],
    "variants": [
        {
            "id": 30111,
            "sku": "000123457",
            "available": true,
            "product_sku": "000123454",
            "face_value": null,
            "product_id": null,
            "voucher_status": false,
            "variant": "small",
            "variant_base_price": 100,
            "face_value_gbp": null
        },
        {
            "id": 30112,
            "sku": "000123459",
            "available": true,
            "product_sku": "000123454",
            "face_value": null,
            "product_id": null,
            "voucher_status": false,
            "variant": "medium",
            "variant_base_price": 100,
            "face_value_gbp": null
        },
        {
            "id": 30113,
            "sku": "000123461",
            "available": true,
            "product_sku": "000123454",
            "face_value": null,
            "product_id": null,
            "voucher_status": false,
            "variant": "large",
            "variant_base_price": 100,
            "face_value_gbp": null
        },
        {
            "id": 30114,
            "sku": "000123463",
            "available": true,
            "product_sku": "000123454",
            "face_value": null,
            "product_id": null,
            "voucher_status": false,
            "variant": "Afleck",
            "variant_base_price": 100,
            "face_value_gbp": null
        }
    ],
    "media": [
        {
            "id": 6419,
            "url": "https://images.com/123533.jpg"
        }
    ],
    "delivery_type": {
        "id": 1,
        "name": "Physical",
        "requires_address": true,
        "requires_email": true
    },
    "currency": {
        "title": "Dollars",
        "abbreviation": "USD",
        "sign": "$"
    }
  },
  {
    ...
  }
]
```


