## Get supplier products

This endpoint will return details for one or more products. This API can only be used by authorized suppliers, and you will only be able to fetch details for products associated with your supplier API key. Base prices will be returned in the product currency and will remain unaltered. 

Results will be returned paginated at 100 items per page, and we encourage you to use the available filters. 

#### HTTP Request

`GET /api/v2/supplier_products`

#### Filter Parameters

Parameter | Type | Info
--------- | ---- | ----
skus | string | A comma separate list of the SKU or SKU's of the product or variant you are looking for. If the item is a variant, we will return the parent product along with all of its variants in the `variants` key
minimum_age | integer | When present only products suitable for a minimum age of this value will be shown - for instance a value of 18 will only list products that have either no minimum age or a value greater then or equal to 18
delivery_types | string | A comma separate list of the methods of delivery e.g. email, downloadable, physical etc.
countries | string | A comma separate list of the names of the available delivery countries  
status | string | A comma separate list of the names, of the product status to return e.g. pending, approved etc



> Request

``` http
GET /api/v2/supplier_products HTTP/1.1
Authorization: Token token=xxx

{
  "skus": "A-123-10"
}
```

> Response

``` http
HTTP/1.1 200 Ok
Content-Type: application/json

{
  "id": 357,
  "name": "Voucher name",
  "base_price": 9.95,
  "description": "<h1>Voucher description</h1><p>Additional description in a paragraph tag</p>",
  "model_no": "model_a-123",
  "sku": "A-123|10",
  "available": true,
  "availability_note": "",
  "international_requirements": false,
  "minimum_age": null,
  "primary_image_id": 337,
  "variant": "",
  "voucher": true,
  "lowest_denomination": "$10",
  "face_value": 10,
  "supplier_currency_id": 41,
  "max_value_per_order": null,
  "available_countries": [
    {
      "id": 140,
      "name": "UK",
      "vat_rate": "0%",
      "delivery_charge": 0
    },
    {
      "id": 165,
      "name": "Albania",
      "vat_rate": "0%",
      "delivery_charge": 0
    }
  ],
  "rrp": "159.99",
  "supplier": {
    "id": 14
  },
  "vat_rate": {
    "name": "0%",
    "numeric": 0
  },
  "status": {
    "name": "Pending"
  },
  "catalogue": {
    "id": 2,
    "name": "Vouchers",
    "exclusive": false
  },
  "categories": [
    {
      "id": 542,
      "name": "Food & Dining",
      "parent_id": 75
    }
  ],
  "variants": [
    {
      "id": 529,
      "variant": "$10",
      "sku": "A-123|10",
      "available": true,
      "product_sku": "A-123|10",
      "face_value": 10,
      "voucher_status": true,
      "variant_base_price": 9.95,
    },
    {
      "id": 530,
      "variant": "$20",
      "sku": "A-123|20",
      "available": true,
      "product_sku": "A-123|10",
      "face_value": 20,
      "voucher_status": true,
      "variant_base_price": 19.95,
    }
  ],
  "media": [
    {
      "id": 337,
      "url": "https://example.test/image.jpg"
    }
  ],
  "delivery_type": {
    "id": 3,
    "name": "Email",
    "requires_address": false
  },
  "currency": {
    "title": "US Dollars",
    "abbreviation": "USD",
    "sign": "$"
  },
  "translations": []
}
```