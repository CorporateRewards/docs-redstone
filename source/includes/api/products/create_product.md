## Create product

You can create products over the API as shown - all new products require approval. Products can only be approved using the GPS web interface by a supplier manager or CR staff. Products can only be approved once they have a Category, a Catalogue and at least one image applied. Once a product is approved, it can be shown in a catalogue and ordered, subject to availability.

The created product has a status of pending, and unless provided in the POST request, will have no catalogue or categories - this product needs to be approved in GPS before it will show in a catalogue

> Request

``` http
POST /api/v2/products HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
	"product": {
		"name": "Bat Utility Belt",
		"model_no": "bub001",
		"brand_id": 1,
		"description": "<p>Quite simply the quintessential superhero utility belt. Iconic and functional - no bat-based superhero should be without it </p>",
		"sku": "000123454",
		"base_price": 100.0,
		"rrp": 100.0,
		"face_value": null,
		"media": "https://images.com/12353.jpg",
		"currency": "USD",
		"availability_note": "",
		"available": "yes",
		"countries": [{
			"country": "USA",
			"vat_rate": 0,
			"delivery_charge": 0
		}],
		"international_requirements": 0,
		"minimum_age": 0,
		"vat_rate_id": 1,
		"delivery_type_id": 1,
		"voucher": false,
		"lowest_denomination": null,
		"catalogue_id": 8,
		"categories": [3,4,12]
		"variants": [
			"{\"available\": 1, \"product_sku\": \"000123454\", \"sku\": \"000123457\", \"face_value\": null, \"base_price\": 100.0, \"voucher_status\": 0, \"variant\":\"small\" }",
			"{\"available\": 1, \"product_sku\": \"000123454\", \"sku\": \"000123459\", \"face_value\": null, \"base_price\": 100.0, \"voucher_status\": 0, \"variant\":\"medium\" }",
			"{\"available\": 1, \"product_sku\": \"000123454\", \"sku\": \"000123461\", \"face_value\": null, \"base_price\": 100.0, \"voucher_status\": 0, \"variant\":\"large\" }",
			"{\"available\": 1, \"product_sku\": \"000123454\", \"sku\": \"000123463\", \"face_value\": null, \"base_price\": 100.0, \"voucher_status\": 0, \"variant\":\"Afleck\" }"
		]
	}
}
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json

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
  "brand_id": 1,
  "name": "Bat Utility Belt",
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

`POST /api/v2/products`

#### Attributes

Attribute | Type | Info
--------- | ---- | ----
name  | String  |  Required - name of the product as it displayed in the catalogue
model_no  | String  |  Optional - short descriptive code or model name
description  | HTML (String)  |  Required - a textual, detailed description of the product can be plain text or marked up in HTML
sku  | String  |  Required - canonical unique reference or identity of a product used for ordering
base_price  | Float  |  Required - this is the base_price excluding tax and delivery - the cost of the product
rrp  | Float  |  Optional - recommended retail price of product
face_value  | Integer  |  Optional conditional - if this product is to be classed as a voucher, this is face value of the lowest denomination
media  | Url (String)  |  Optional - a publicly accessible URL for an image (JPG or PNG). This will be fetched by our system, transcoded and associated with this product as the primary image. This image will also be placed in the media library for your supplier in GPS
currency  | String  |  Required - ISO 3 letter currency code for the currency this product will be billed in - this currency must be associated with your supplier record
availability_note  | String  |  Optional - this text will be displayed in place of an order button to explain why the product is not available such as "Coming soon" or "Temporarily out of stock"
available  | Boolean  |  Required - this indicates the product is or isn't available and triggers the availability_note
countries  | Array  |  Required - a list of at least one country this product is available to order in
countries.country  | String  |  Required - 3 letter ISO country code - this country must be associated with your supplier
countries.vat_rate  | Float  |  the rate of sales tax that will be used to bill for the delivery of this product
countries.delivery_charge  | Float  |  The charge to be billed for shipping this product excluding taxes
international_requirements  | Boolean  |  Required - indicates if this product has international requirements or variations such as alternative plugs or instructions. Not shown to end users, more intended for order fulfilment
minimum_age  | Integer  |  Required - indicates if a minimum age is legally required for this product such as alcohol
vat_rate_id  | Integer  |  Required - this indicates a sales tax rate. Note this is not a value but a reference to a sales tax rate stored in GPS
delivery_type_id  | Integer  |  Required - delivery types in GPS are (but not limited to) Physical = 1, Downloadable = 2, Email = 3, Prepaid = 4. These types are used to enforce validation strategies on orders for these products i.e. for physical orders, you must provide a postal address
voucher  | Boolean  |  Required - indicates if this product is to be treated as a voucher
lowest_denomination  | Integer  | Optional conditional - if this product is considered a voucher this field should indicate the lowest denomination of voucher available
catalogue_id  | Integer  |  Optional - The id of the catalogue the product should belong to
categories  | Array  |  Optional - An array of category ids that the product should belong to
variants  | Array  |  Required - this is (somewhat convolutedly) a list of at 0 or more JSON encoded strings representing objects for a variant
variants.available  | Boolean  |  Required - indicates the availability of this particular variant
variants.product_sku  | String  |  Required - this is a 'key' that links this variant to its parent product, the value should be the SKU of the main product we are posting
variants.sku  | String  |  Required - this is the SKU of this particular variant and should be unique
variants.face_value  | Integer  |  Required conditional - if this product is a voucher, this shows the face value of this denomination
variants.base_price  | Float  |  Required conditional - for non-voucher products all variants will have the same cost/base price but voucher denominations will have differing base prices
variants.voucher_status  | Boolean  |  Required - indicated this variant is a voucher - you cannot mix vouchers and physical product variants this should be all or nothing and match the parent product
variants.variant  | String  |  Required - for non voucher products this represents the reason for the variation such as size (medium large etc) or colour. For vouchers this represents the face value of this denomination
brand_id  | Integer  | Optional - this indicates a brand. Note this is not the name of the brand, but the id of the brand as stored in GPS
