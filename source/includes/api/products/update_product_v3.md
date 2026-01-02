## Update product

V3 of the update product API is available to update product and product variant information. Your product may have variants (e.g. size: small|medium|large, colour: blue|red|black) or denominations in the case of vouchers (e.g. $10, $20, $50).


#### HTTP Request

`PATCH /api/v3/products/:product_id` - We will only update the data that is sent and therefore you may send only data that is to be changed. Where the value being sent is an array of resources (e.g. countries and variants), you must send all items in the collection. If a country association or variant exists against a product and it is not sent in the array, then the item will be deleted. You may send only the country (GB, US, CA etc) or the SKU where no change is required, otherwise you should send the full object. If you do not send the countries or variants key at all, then they will remain unchanged as with all of other keys. 

#### Country and Variant examples

Original data

``` json
{
  "name": "Voucher name",
  "base_price": 9.95,
  [..............],
  "countries": [
    {
      "country": "GB",
      "vat_rate": "20%",
      "delivery_charge": 1
    },
    {
      "country": "US",
      "vat_rate": 0,
      "delivery_charge": 0
    },
    {
      "country": "DE",
      "vat_rate": 0,
      "delivery_charge": 0
    }
  ],
  "variants": [
    {
      "sku": "A-123|20",
      "variant": "$20",
      "face_value": 20,
      "base_price": 19.95,
      "available": true
    },
    {
      "sku": "A-123|30",
      "variant": "$30",
      "face_value": 30,
      "base_price": 29.95,
      "available": true
    },
    {
      "sku": "A-123|100",
      "variant": "$100",
      "face_value": 100,
      "base_price": 99.95,
      "available": false
    }
  ]
}
```

> Request
``` json
PATCH /api/v3/products/:product_id HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
  "name": "Updated voucher name",
  "countries": [
    {
      "country": "GB" // Will remain unchanged
    },
    {
      "country": "US",
      "vat_rate": 0, // No change
      "delivery_charge": 5.50 // Will change on country US to 5.50
    },
    {
        "country": "CA", // Will be added as a new associated country
        "vat_rate": "20%"
        "delivery_change": 8.00
    }
  ], // Note "country": "DE" has not been sent and will be deleted
  "variants": [
    {
      "sku": "A-123|20" // Will remain unchanged
    },
    {
      "sku": "A-123|30",
      "variant": "$30",
      "face_value": 30,
      "base_price": 29.95,
      "available": false // Will be changed to unavailable
    },
    {
      "sku": "A-123|50", // Will be added
      "variant": "$50",
      "face_value": 50,
      "base_price": 49.95,
      "available": true
    }
  ] // Note "sku": "A-123|100" has not been sent and will be deleted
}
```

> Response

``` json
{
    "id": 42223,
    "name": "Updated voucher name",
    "base_price": 9.95,
    "description": "<p>A voucher for spending at our store</p>",
    "model_no": "voucher123",
    "sku": "A-123|10",
    "available": true,
    "availability_note": null,
    "international_requirements": false,
    "minimum_age": 0,
    "primary_image_id": null,
    "variant": null,
    "voucher": true,
    "lowest_denomination": "2",
    "face_value": 2,
    "supplier_currency_id": 460,
    "max_value_per_order": "1000.0",
    "available_countries": [
        {
            "id": 140,
            "name": "UK",
            "vat_rate": "20%",
            "delivery_charge": 1.00
        },
        {
            "id": 207,
            "name": "US",
            "vat_rate": "0%",
            "delivery_charge": 5.50
        },
        {
            "id": 212,
            "name": "CA",
            "vat_rate": "20%",
            "delivery_charge": 8.00
        }
    ],
    "rrp": "10",
    "supplier": {
        "id": 184
    },
    "brand": {
        "id": 15,
        "name": "My Brand"
    },
    "variant_type": null,
    "vat_rate": {
        "name": "0%",
        "numeric": 0.0
    },
    "status": {
        "name": "Approved"
    },
    "catalogue": {
        "id": 2,
        "name": "Vouchers",
        "exclusive": false
    },
    "categories": [
        {
            "id": 98,
            "name": "Online Retailers",
            "parent_id": 75
        },
        {
            "id": 341,
            "name": "Toys & Games",
            "parent_id": 336
        }
    ],
    "variants": [
        {
            "id": 205496,
            "variant": "£10",
            "sku": "A-123|10",
            "translations": [],
            "available": true,
            "product_sku": "A-123|10",
            "face_value": 10,
            "product_id": null,
            "voucher_status": true,
            "variant_base_price": 10.0,
            "face_value_gbp": 10.0
        },
        {
            "id": 205498,
            "variant": "£20",
            "sku": "A-123|20",
            "translations": [],
            "available": true,
            "product_sku": "A-123|10",
            "face_value": 20,
            "product_id": null,
            "voucher_status": true,
            "variant_base_price": 20.0,
            "face_value_gbp": 20.0
        },
        {
            "id": 205499,
            "variant": "£30",
            "sku": "A-123|30",
            "translations": [],
            "available": false,
            "product_sku": "A-123|10",
            "face_value": 30,
            "product_id": null,
            "voucher_status": true,
            "variant_base_price": 30.0,
            "face_value_gbp": 30.0
        },
        {
            "id": 205510,
            "variant": "£50",
            "sku": "A-123|50",
            "translations": [],
            "available": true,
            "product_sku": "A-123|10",
            "face_value": 50,
            "product_id": null,
            "voucher_status": true,
            "variant_base_price": 50.0,
            "face_value_gbp": 50.0
        }
    ],
    "media": [],
    "delivery_type": {
        "id": 3,
        "name": "Email",
        "requires_address": false
    },
    "currency": {
        "title": "Pounds Sterling",
        "abbreviation": "GBP",
        "sign": "£"
    },
    "translations": []
}
```


#### Request Parameters

Attribute | Type | Info
--------- | ---- | ----
name  | String  |  Optional - name of the product as it displayed in the catalogue
model_no  | String  |  Optional - short descriptive code or model name
description  | HTML (String)  |  Optional - a textual, detailed description of the product can be plain text or marked up in HTML
sku  | String  |  Optional - canonical unique reference or identity of a product used for ordering
base_price  | Float  |  Optional - this is the base_price excluding tax and delivery - the cost of the product
rrp  | Float  |  Optional - recommended retail price of product
face_value  | Integer  |  Conditional - if this product is classed as a voucher, this value is required and is the face value of the lowest denomination
media  | URL (String)  |  Optional - a publicly accessible URL for an image (jpg or png). This will be fetched by our system, transcoded and associated with this product as the primary image. This image will also be placed in the media library for your supplier in GPS
currency  | String  |  Optional - ISO currency code for the currency this product will be billed in - this currency must be associated with your supplier record
availability_note  | String  |  Required conditional - this text will be displayed in GPS only and informs our team why the product is not available such as "Coming soon" or "Temporarily out of stock". If 'available' is sent as false then you ust provide an availabilty note.
available  | Boolean  |  Optional - this indicates the product is or isn't available and triggers the availability_note
countries  | Array  |  Optional - a list of countries this product is available to order in. You can add countries but not remove them via the API.
countries.country  | String  |  Required for each country added - ISO country code - this country must be associated with your supplier
countries.vat_rate  | Float  |  Required for each country added - the rate of sales tax that will be used to bill for the delivery of this product
countries.delivery_charge  | Float  |  Required for each country added - The charge to be billed for shipping this product excluding taxes
international_requirements  | Boolean  |  Optional - indicates if this product has international requirements or variations such as alternative plugs or instructions. Not shown to end users, more intended for order fulfilment
minimum_age  | Integer  |  Optional - indicates if a minimum age is legally required for this product such as alcohol
vat_rate_id  | Integer  |  Optional - this indicates a sales tax rate. Note this is not a value but a reference to a sales tax rate stored in GPS
delivery_type_id  | Integer  |  Optional - delivery types in GPS are (but not limited to) Physical = 1, Downloadable = 2, Email = 3, Prepaid = 4. These types are used to enforce validation strategies on orders for these products i.e. for physical orders, you must provide a postal address
voucher  | Boolean  |  Optional - indicates if this product is to be treated as a voucher
lowest_denomination  | Integer  | Optional conditional - if this product is considered a voucher this field should indicate the lowest denomination of voucher available
catalogue_id  | Integer  |  Optional - The id of the catalogue the product should belong to
variants | Array | Optional - an array of variants or denominations for this product
variants.variant | String | Required - for non-voucher products this represents the reason for the variation such as size (medium large etc) or colour. For vouchers this represents the face value of this denomination
variants.sku | String | Required - this is the SKU of this particular variant and should be unique
variants.face_value | Integer | Required (conditional) - if this product is a voucher, this shows the face value of this denomination
variants.base_price | Float | Required (conditional) - for non-voucher products all variants will have the same cost/base price but voucher denominations will have differing base prices
variants.available | Boolean | Optional (default true) - indicates the availability of this particular variant
brand_id  | Integer  | Optional - this indicates a brand. Note this is not the name of the brand, but the id of the brand as stored in GPS


### Examples

#### Voucher with denominations

> Request

``` http
PATCH /api/v3/products/:product_id HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
    "product": {
        "name": "Updated Store Voucher",
        "model_no": "store10_model",
        "sku": "store10",
        "variants": [
            {
                "sku": "store20",
                "variant": "£20",
                "face_value": 20,
                "base_price": 20,
                "available": false
            },
            {
                "sku": "store50",
                "variant": "£50",
                "face_value": 50,
                "base_price": 50,
                "available": true
            }
        ],
    }
}
```

``` http
PATCH /api/v3/products/:product_id HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
    "product": {
        "name": "Updated product",
        "model_no": "product_model",
        "sku": "product123",
        "countries": [
            {
                "country": "GB",
                "vat_rate": "20%",
                "delivery_charge": 5.00
            },
            {
                "country": "US",
                "delivery_charge": 30.00
            }
        ]
    }
}
```

> Request

``` http
PUT /api/v3/products/:product_id HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
  "name": "Voucher name",
  "base_price": 9.95,
  "description": "<h1>Voucher description</h1><p>Additional description in a paragraph tag</p>",
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
      "country": "US",
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
      "sku": "A-123|20",
      "variant": "$20",
      "face_value": 20,
      "base_price": 19.95,
      "available": true
    },
    {
      "sku": "A-123|30",
      "variant": "$30",
      "face_value": 30,
      "base_price": 29.95,
      "available": true
    }
  ],
  "media": ["https://example.test/image.jpg"],
  "delivery_type_id": 3,
  "currency": "USD"
}
```


> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "id": 357,
  "name": "Voucher name",
  "base_price": 158.27,
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

#### Product with variants


> Request

``` http
PATCH /api/v3/products/:product_id HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
    "product": {
        "name": "Updated Store Voucher",
        "model_no": "store10_model",
        "sku": "store10",
        "variants": [
            {
                "sku": "store20",
                "variant": "£20",
                "face_value": 20,
                "base_price": 20,
                "available": false
            },
            {
                "sku": "store50",
                "variant": "£50",
                "face_value": 50,
                "base_price": 50,
                "available": true
            }
        ],
    }
}
```


> Request

``` http
PUT /api/v3/products/:product_id HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
  "name": "Voucher name",
  "base_price": 9.95,
  "description": "<h1>Voucher description</h1><p>Additional description in a paragraph tag</p>",
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
      "country": "US",
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
      "sku": "A-123|20",
      "variant": "$20",
      "face_value": 20,
      "base_price": 19.95,
      "available": true
    },
    {
      "sku": "A-123|30",
      "variant": "$30",
      "face_value": 30,
      "base_price": 29.95,
      "available": true
    }
  ],
  "media": ["https://example.test/image.jpg"],
  "delivery_type_id": 3,
  "currency": "USD"
}
```


> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "id": 357,
  "name": "Voucher name",
  "base_price": 158.27,
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