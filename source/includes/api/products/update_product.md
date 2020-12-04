## Update product

You can update products over the api as shown. If passing categories or variants you must provide the full list of categories or variants required, as this will recreate categories or variants as per the new array provided. As such, passing an empty array to categories or variants will delete the existing categories or variants.

```
PATCH /api/v2/products/:id
Authorization: Token token=xxx

{
	"product": {
		"name": "Your updated product name",
		"description": "<p>Your updated product description</p>",
	    "variants": [
	      "{\"available\": 1, \"product_sku\": \"PRODUCTSKU\", \"sku\": \"000123457\", \"face_value\": null, \"base_price\": 100.0, \"voucher_status\": 0, \"variant\":\"small\" }",
	      "{\"available\": 1, \"product_sku\": \"PRODUCTSKU\", \"sku\": \"000123459\", \"face_value\": null, \"base_price\": 100.0, \"voucher_status\": 0, \"variant\":\"medium\" }",
	      "{\"available\": 1, \"product_sku\": \"PRODUCTSKU\", \"sku\": \"000123461\", \"face_value\": null, \"base_price\": 100.0, \"voucher_status\": 0, \"variant\":\"large\" }"  
	      ]
	}
}
```

```
HTTP/1.1 200 OK
Content-Type: application/json

{
    "id": 7833,
    "base_price": 115.6932,
    "model_no": "MODELNUMBER",
    "sku": "PRODUCTSKU",
    "available": true,
    "availability_note": "",
    "international_requirements": false,
    "minimum_age": null,
    "primary_image_id": null,
    "variant": "",
    "voucher": false,
    "lowest_denomination": null,
    "face_value": null,
    "supplier_currency_id": 47,
    "name": "Your updated product name",
    "description": "<p>Your updated product description</p>",
    "available_countries": [],
    "rrp": "23131",
    "supplier": {
        "id": 49
    },
    "vat_rate": {
        "name": "10.45%",
        "numeric": 10.45
    },
    "status": {
        "name": "Pending"
    },
    "categories": [],
    "variants": [
        {
            "id": 1412,
            "sku": "000123457",
            "available": true,
            "product_sku": "PRODUCTSKU",
            "face_value": null,
            "product_id": null,
            "voucher_status": false,
            "variant": "small",
            "variant_base_price": 93.3009,
            "face_value_gbp": null
        },
        {
            "id": 1413,
            "sku": "000123459",
            "available": true,
            "product_sku": "PRODUCTSKU",
            "face_value": null,
            "product_id": null,
            "voucher_status": false,
            "variant": "medium",
            "variant_base_price": 93.3009,
            "face_value_gbp": null
        },
        {
            "id": 1414,
            "sku": "000123461",
            "available": true,
            "product_sku": "PRODUCTSKU",
            "face_value": null,
            "product_id": null,
            "voucher_status": false,
            "variant": "large",
            "variant_base_price": 93.3009,
            "face_value_gbp": null
        }
    ],
    "media": [],
    "delivery_type": {
        "id": 1,
        "name": "Physical",
        "requires_address": true,
        "requires_email": false
    },
    "currency": {
        "title": "Euro",
        "abbreviation": "EUR",
        "sign": "€"
    }
}
```

Attribute | Type | Info
--------- | ---- | ----
name  | String  |  Optional - name of the product as it displayed in the catalogue
model_no  | String  |  Optional - short descriptive code or model name
description  | Html (String)  |  Optional - a textual, detailed description of the product can be plain text or marked up in html
sku  | String  |  Optional - canonical unique reference or identity of a product used for ordering
base_price  | Float  |  Optional - this is the base_price excluding tax and delivery - the cost of the product
rrp  | Float  |  Optional - recommended retail price of product
face_value  | Integer  |  Required conditional - if this product is to be classed as a voucher, this is face value of the lowest denomination
media  | Url (String)  |  Optional - a publicly accessible url for an image (jpg or png). This will be fetched by our system, transcoded and associated with this product as the primary image. This image will also be placed in the media library for your supplier in GPS
currency  | String  |  Optional - ISO 3 letter currency code for the currency this product will be billed in - this currency must be associated with your supplier record
availability_note  | String  |  Optional - this text will be displayed in place of an order button to explain why the product is not available such as "Coming soon" or "Temporarily out of stock"
available  | Boolean  |  Optional - this indicates the product is or isn't available and triggers the availability_note
countries  | Array  |  Optional - a list of at least one country this product is available to order in
countries.country  | String  |  Optional - 3 letter ISO country code - this country must be associated with your supplier
countries.vat_rate  | Float  |  the rate of sales tax that will be used to bill for the delivery of this product
countries.delivery_charge  | Float  |  The charge to be billed for shipping this product excluding taxes
international_requirements  | Boolean  |  Optional - indicates if this product has international requirements or variations such as alternative plugs or instructions. Not shown to end users, more intended for order fulfilment
minimum_age  | Integer  |  Optional - indicates if a minimum age is legally required for this product such as alcohol
vat_rate_id  | Integer  |  Optional - this indicates a sales tax rate. Note this is not a value but a reference to a sales tax rate stored in GPS
delivery_type_id  | Integer  |  Optional - delivery types in GPS are (but not limited to) Physical = 1, Downloadable = 2, Email = 3, Prepaid = 4. These types are used to enforce validation strategies on orders for these products i.e. for physical orders, you must provide a postal address
voucher  | Boolean  |  Optional - indicates if this product is to be treated as a voucher
lowest_denomination  | Integer  | Required conditional - if this product is considered a voucher this field should indicate the lowest denomination of voucher available
catalogue_id  | Integer  |  Optional - The id of the catalogue the product should belong to
categories  | Array  |  Optional - An array of category ids that the product should belong to
variants  | Array  |  Optional - this is (somewhat convolutedly) a list of at 0 or more JSON encoded strings representing objects for a variant
variants.available  | Boolean  |  Optional - indicates the availability of this particular variant
variants.product_sku  | String  |  Optional - this is a 'key' that links this variant to its parent product, the value should be the sku of the main product we are posting
variants.sku  | String  |  Optional - this is the sku of this particular variant and should be unique
variants.face_value  | Integer  |  Required conditional - if this product is a voucher, this shows the face value of this denomination
variants.base_price  | Float  |  Required conditional - for non-voucher products all variants will have the same cost/base price but voucher denominations will have differing base prices
variants.voucher_status  | Boolean  |  Optional - indicated this variant is a voucher - you cannot mix vouchers and physical product variants this should be all or nothing and match the parent product
variants.variant  | String  |  Optional - for non voucher products this represents the reason for the variation such as size (medium large etc) or colour. For vouchers this represents the face value of this denomination
