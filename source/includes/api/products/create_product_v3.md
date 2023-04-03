### V3

> Request (non-voucher example)

``` http
POST /api/v3/products HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
  "name": "Product Name",
  "base_price": 158.27,
  "description": "Product Description",
  "model_no": "A-123",
  "sku": "A-123",
  "available": true,
  "voucher": false,
  "countries": [
    {
      "country": "GB",
      "vat_rate": 20.0,
      "delivery_charge": 7.2
    },
    {
      "country": "AL",
      "vat_rate": 17.5,
      "delivery_charge": 34.75
    }
  ],
  "rrp": "159.99",
  "vat_rate": 20,
  "brand_id": 1,
  "catalogue_id": 8,
  "categories": [62],
  "variant_type_id": 2,
  "variants": [
    {
      "variant": "small",
      "sku": "B-123",
      "available": true
    },
    {
      "variant": "large",
      "sku": "B-234",
      "available": true
    }
  ],
  "media": ["https://example.test/image.jpg"],
  "delivery_type_id": 1,
  "currency": "GBP"
}
```

> Response (non-voucher example)

``` http
HTTP/1.1 201 Created
Content-Type: application/json

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
  "max_value_per_order": null,
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
      "vat_rate": "17.5%",
      "delivery_charge": 34.75
    }
  ],
  "rrp": "159.99",
  "supplier": {
    "id": 14
  },
  "brand": {
    "id": 1,
    "name": "Acme"
  },
  "variant_type": {
    "id": 2,
    "name": "Size"
  },
  "vat_rate": {
    "name": "20%",
    "numeric": 20
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
      "id": 62,
      "name": "Necklaces",
      "parent_id": 59
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
      "url": "https://example.test/image.jpg"
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
  "translations": []
}
```

> Request (voucher example)

``` http
POST /api/v3/products HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
  "name": "Product Name",
  "base_price": 9.95,
  "description": "Product Description",
  "sku": "A-123|10",
  "available": true,
  "voucher": true,
  "face_value": 10,
  "lowest_denomination": "$10",
  "countries": [
    {
      "country": "GB",
      "vat_rate": 0,
      "delivery_charge": 0
    },
    {
      "country": "AL",
      "vat_rate": 0,
      "delivery_charge": 0
    }
  ],
  "rrp": "$10",
  "vat_rate": 0,
  "catalogue_id": 2,
  "categories": [542],
  "variants": [
    {
      "variant": "$10",
      "face_value": 10,
      "base_price": 9.95,
      "sku": "A-123|10",
      "available": true
    },
    {
      "variant": "$20",
      "face_value": 20,
      "base_price": 19.95,
      "sku": "A-123|20",
      "available": true
    }
  ],
  "media": ["https://example.test/image.jpg"],
  "delivery_type_id": 3,
  "currency": "USD"
}
```

> Response (voucher example)

``` http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 357,
  "name": "Product Name",
  "base_price": 158.27,
  "description": "Product Description",
  "model_no": null,
  "sku": "A-123",
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
      "product_id": null,
      "voucher_status": true,
      "variant_base_price": 9.95,
      "face_value_gbp": null
    },
    {
      "id": 530,
      "variant": "$20",
      "sku": "A-123|20",
      "available": true,
      "product_sku": "A-123|10",
      "face_value": 20,
      "product_id": null,
      "voucher_status": true,
      "variant_base_price": 19.95,
      "face_value_gbp": null
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
#### HTTP Request

`POST /api/v3/products`

#### Attributes

Attribute | Type | Info
--------- | ---- | ----
name | String | Required - name of the product as it is displayed in the catalogue
description | String | Required - a textual, detailed description of the product - can be plain text or HTML
sku | String | Required - canonical unique reference or identity of a product used for ordering
base_price | Float | Required - this is the `base_price` excluding tax and delivery - the cost of the product
rrp | String | Required - recommended retail price of product
currency | String | Required - ISO 3-letter currency code for the currency this product will be billed in - this currency must be associated with your supplier 
available | Boolean | Required - this indicates if the product is or isn't available and triggers the `availability_note` if false
vat_rate | Float | Required - this indicates a sales tax rate
delivery_type_id | Integer | Required - delivery types in GPS are (but not limited to) Physical = 1, Downloadable = 2, Email = 3, Prepaid = 4. These types are used to enforce validation strategies on orders for these products - i.e. for physical orders, you must provide a postal address
voucher | Boolean | Optional (default false) - indicates if this product is to be treated as a voucher
model_no | String | Optional - short descriptive code or model name
face_value | Integer | Required (conditional) - if this product is to be classed as a voucher, this is the face value of the lowest denomination
media | Array of URLs (String) | Optional - publicly accessible URL(s) for image(s) (JPG or PNG). This will be fetched by our system, transcoded and associated with this product, with the first URL as the primary image. Images will also be placed in the media library for your supplier in GPS
availability_note | String | Optional - this text will be displayed in GPS only to inform our team why the product is not available such as "Coming soon" or "Temporarily out of stock"
countries | Array | Optional - a list of at least one country this product is available to order in
countries.country | String | Required - 2-letter ISO country code - this country must be associated with your supplier
countries.vat_rate | Float | Required - the rate of sales tax that will be used to bill for the delivery of this product
countries.delivery_charge | Float | Required - the charge to be billed for shipping this product, excluding taxes
international_requirements | Boolean | Optional (default false) - indicates if this product has international requirements or variations such as alternative plugs or instructions. Not shown to end users, more intended for order fulfilment
minimum_age | Integer | Optional - indicates if a minimum age is legally required for this product (as with alcohol etc.)
max_value_per_order | Float | Optional - the maximum total value (`quantity * face_value` for vouchers, `quantity * base_price` otherwise) of this product that can be purchased in a single order
lowest_denomination | String | Required (conditional) - if this product is considered a voucher, this field should indicate the lowest denomination of voucher available
brand_id | Integer | Optional - this indicates a brand. Note this is not the name of the brand, but the ID of the brand as stored in GPS
catalogue_id | Integer | Optional - the ID of the catalogue the product should belong to
categories | Array of Integers | Optional - an array of category IDs that the product should belong to
variant_type_id | Integer | Optional - the ID of a variant type in GPS (Colour = 1, Size = 2, Other = 8). Ignored for vouchers (since their variants are always denominations)
variants | Array | Optional - an array of variants or denominations for this product
variants.variant | String | Required - for non-voucher products this represents the reason for the variation such as size (medium large etc) or colour. For vouchers this represents the face value of this denomination
variants.sku | String | Required - this is the SKU of this particular variant and should be unique
variants.face_value | Integer | Required (conditional) - if this product is a voucher, this shows the face value of this denomination
variants.base_price | Float | Required (conditional) - for non-voucher products all variants will have the same cost/base price but voucher denominations will have differing base prices
variants.available | Boolean | Optional (default true) - indicates the availability of this particular variant
