## Update product

The update product API is available to update product information.

> Request

``` http
PATCH /api/v2/products/{product_id} HTTP/1.1
Authorization: Token token={KEY}:{SECRET}
Content-Type: application/json

{
    "product": {
        "name": "Updated product",
        "model_no": "735updated",
        "description": "<p>A high quality updated product</p>",
        "sku": "DEMO - updated",
        "base_price": 30,
        "rrp": 45,
        "face_value": null,
        "media": "https://images.com/12353.jpg",
        "currency": "USD",
        "availability_note": "available",
        "available": "yes",
        "countries": [{
            "country": "US",
            "vat_rate": 0,
            "delivery_charge": 0
        }
        ],
        "international_requirements": 0,
        "minimum_age": 21,
        "vat_rate_id": 1,
        "delivery_type_id": 1,
        "voucher": false,
        "lowest_denomination": null,
        "catalogue_id": 1,
        "brand_id": 1,
        "variants": ["{\"available\": 1, \"product_sku\": \"000123454\", \"sku\": \"000123457\", \"face_value\": null, \"base_price\": 100.0, \"voucher_status\": 0, \"variant\":\"test\" }"]
    }
}
```

> Response

```
HTTP/1.1 200 OK
Content-Type: application/json
{
    "id": 123,
    "base_price": 30,
    "model_no": "735updated",
    "sku": "DEMO - updated",
    "available": true,
    "availability_note": "available",
    "international_requirements": false,
    "minimum_age": 21,
    "primary_image_id": 6419,
    "variant": null,
    "voucher": false,
    "lowest_denomination": null,
    "face_value": null,
    "supplier_currency_id": 123,
    "max_value": null,
    "brand_id": 1,
    "name": "Updated product",
    "description": "<p>A high quality updated product</p>",
    "available_countries": [
        {
            "id": 120,
            "name": "USA",
            "vat_rate": "0%",
            "delivery_charge": 0.0
        }
    ],
    "rrp": "45",
    "supplier": {
        "id": 1
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
        "name": "Core merch",
        "exclusive": false
    },
    "categories": [
        {
            "id": 47,
            "parent_id": 4,
            "name": "Test"
        }
    ],
    "variants": [
        {
            "id": 1655,
            "sku": "000123457",
            "available": true,
            "product_sku": "DEMO - 73568updated",
            "face_value": null,
            "product_id": null,
            "voucher_status": false,
            "variant": "test",
            "variant_base_price": 78.2962,
            "face_value_gbp": null
        }
    ],
    "media": [
        {
            "id": 123,
            "url": "https://images.com/123533.jpg"
        }
    ],
    "delivery_type": {
        "id": 1,
        "name": "Physical",
        "requires_address": true,
        "requires_email": false
    },
    "variant_type": {
        "id": 10,
        "name": "other"
    },
    "currency": {
        "title": "US Dollar",
        "abbreviation": "USD",
        "sign": "$"
    }
}
```

#### HTTP Request
`PUT /api/v2/products/{product_id}`

`PATCH /api/v2/products/{product_id}`

#### Request Parameters

Attribute | Type | Info
--------- | ---- | ----
name  | String  |  Optional - name of the product as it displayed in the catalogue
model_no  | String  |  Optional - short descriptive code or model name
description  | HTML (String)  |  Optional - a textual, detailed description of the product can be plain text or marked up in HTML
sku  | String  |  Optional - canonical unique reference or identity of a product used for ordering
base_price  | Float  |  Optional - this is the base_price excluding tax and delivery - the cost of the product
rrp  | Float  |  Optional - recommended retail price of product
face_value  | Integer  |  Optional conditional - if this product is to be classed as a voucher, this is face value of the lowest denomination
media  | URL (String)  |  Optional - a publicly accessible URL for an image (jpg or png). This will be fetched by our system, transcoded and associated with this product as the primary image. This image will also be placed in the media library for your supplier in GPS
currency  | String  |  Optional - ISO currency code for the currency this product will be billed in - this currency must be associated with your supplier record
availability_note  | String  |  Optional - this text will be displayed in place of an order button to explain why the product is not available such as "Coming soon" or "Temporarily out of stock"
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
variants  | Array  |  Optional - this is (somewhat convolutedly) a list of at 0 or more JSON encoded strings representing objects for a variant
variants.available  | Boolean  |  Required for each variant added - indicates the availability of this particular variant
variants.product_sku  | String  |  Required for each variant added - this is a 'key' that links this variant to its parent product, the value should be the sku of the main product we are posting
variants.sku  | String  |  Required for each variant added - this is the sku of this particular variant and should be unique
variants.face_value  | Integer  |  Required for each variant added - conditional - if this product is a voucher, this shows the face value of this denomination
variants.base_price  | Float  |  Required for each variant added - conditional - for non-voucher products all variants will have the same cost/base price but voucher denominations will have differing base prices
variants.voucher_status  | Boolean  |  Required for each variant added - indicated this variant is a voucher - you cannot mix vouchers and physical product variants this should be all or nothing and match the parent product
variants.variant  | String  |  Required for each variant added - for non voucher products this represents the reason for the variation such as size (medium large etc) or colour. For vouchers this represents the face value of this denomination
brand_id  | Integer  | Optional - this indicates a brand. Note this is not the name of the brand, but the id of the brand as stored in GPS
