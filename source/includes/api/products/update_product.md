## Update product

The update product API is available to update product information. This uses the same params as the [Create product API](https://docs.gps.my-rewards.co.uk/#products-create-product) above.

> Request:

``` http
PATCH /api/v2/products/{product_id} HTTP/1.1
Authorization: Token token={KEY}:{SECRET}
Content-Type: application/json

{
  "product": {
    "name": "Updated Product",
    "model_no": "test_model"
  }
}
```

> Response

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "base_price": 100.0,
  "model_no": "test_model",
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
  "name": "Updated Product",
  "description": "<p>Quite simply the quintessential superhero utility belt. Iconic and functional - no bat-based superhero should be without it </p>",
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
      "name": "Pending"
  },
  "catalogue": {
      "id": 8,
      "name": "Demo Catalogue",
      "exclusive": false
  },
  "categories": [
      {
          "id": 3,
          "parent_id": 1,
          "name": "Experiences & Days Out"
      },
      {
          "id": 4,
          "parent_id": 1,
          "name": "Food and Drink"
      },
      {
          "id": 12,
          "parent_id": 11,
          "name": "Home Entertainment"
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
}
```

#### HTTP Request
`PUT /api/v2/products/{product_id}`

`PATCH /api/v2/products/{product_id}`
