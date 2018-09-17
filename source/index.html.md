---
title: GPS - API Reference

language_tabs:
- http

search: true
---

# Introduction
Welcome to the GPS API documentation.

GPS – the Global Product System from Corporate Rewards Ltd exposes a RESTful JSON api over HTTPS.
For information on how RESTful APIs are designed, how they work and what benefits they offer over more traditional styles of API, see <https://en.wikipedia.org/wiki/Representational_state_transfer>
For more information about JSON, what it looks like and how to use it, see <http://www.json.org/> or <https://en.wikipedia.org/wiki/JSON>

There are two principle use-cases for accessing this system using the API, Ordering systems and Supplier systems. Ordering systems consume product information for the purposes of displaying products to an end user. Ordering systems can then place orders for these products through the API. Orders are processed by GPS and passed to suppliers who then fulfil these orders. Updates to orders, their status and related information will come back to Ordering systems over the API.

| Endpoint | Use case |
| -------- | ------- |
| GET [/api/v2/categories](#list-all-orders) | Orderer |
| GET [/api/v2/regions](#regions)  | Orderer  |
| GET [/api/v2/catalogues](#catalogues)  | Orderer  |
| GET [/api/v2/catalogues/{catalogue_id}/products](#list-all-products-in-a-catalogue) | Orderer  |
| GET [/api/v2/orders](#orders)  | Orderer & Supplier |
| POST [/api/v2/orders](#creating-orders)  | Orderer |
| POST [/api/v2/orders/{order_id}/accept](#accept) | Supplier |
| POST [/api/v2/orders/{order_id}/reject](#reject) | Supplier |
| POST [/api/v2/orders/{order_id}/dispatch](#dispatch) | Supplier |

Product information can be quite dynamic in that Suppliers can modify the data for a product (such as media/images, prices, availability etc.) at any time. We recommend pulling or refreshing the product data approximately every 24 hours. It is typical for an ordering system to maintain a local store of some sort (cache, database tables etc.) for this product data to allow end users to access this data efficiently.

If a product is made unavailable between the times that data is refreshed, then orders for this product will fail with an appropriate error message indicating why the order failed.
Order information can also be pulled through the API to periodically update order information, again, it is recommended that the Ordering system keep some kind of local store of the order information which can be periodically updated (some Ordering systems can poll as much as once an hour to refresh order related information).

There is an additional, optional method for getting updates to orders called “Pingback” which is documented in the ordering section. This method offers “near real-time” order updates for the ordering system that placed the order, that is to say that when an order is updated with GPS a call will be made to the ordering system - this is a prompt for the ordering system to fetch updated order information.


The first thing you will have to find out is the correct API endpoint to use for
the right environment.

-   **Staging:** [https://greenstone.my-rewards.co.uk](http://greenstone.my-rewards.co.uk)
-   For production access, please contact your Corporate Rewards representative

A programme can have one or more API keys, each of which will be granted
permission to access different functionality from the API. As a standard, we use
RESTful json endpoints that will accept either HTML/HTTP form data or json data,
HTML/HTTP is preferred.

# Authentication

In order to use our API endpoints, you will need to have an API token created and
for this token to be granted the relevant permissions. To authenticate requests we
require you to pass us this token in the form of an HTTP header called
`Authorization` with the value of `Token token=YOURAPITOKENHERE`.

<aside class="warning">You must set the <strong>Authorization</strong> as detailed above</aside>

Failure to supply a recognised key in the correct format will result in a 403 HTTP response code and an error message:
HTTP Token: Access denied.

Your api token should have the following format consisting of an api key followed by a api secret, separated with a colon like so:
abcdefghijklmnopqrst:uvwxyz1234567890abcdefghijklmnop

# Typical process flow

## Ordering systems

The typical process flow indicates how to consume information from this API, the order in which it makes sense to do this and some suggestions on how to make efficient use of this service.

Categories are fairly stable and do not change too often. We recommend checking this endpoint and synchronising the information from here to your local store at most once per day and at least once per month. Ordering systems are free to organise the product data however they wish - these categories are provided as suggestions and are actively managed but are not mandatory.

The typical process for fetching product information is to iterate over each available Catalogue, fetching the products for each one. Check to see if you have a local copy of each Product, if the data for the Product is new or has changed since the last sync, store this Product data. If at the end of the list there are local Products you have not seen in the latest sync of Product data, they should be removed from the local store.

In order to find a list of products to show an end user, the ordering system should be able to find products by country. A user should therefore be able to see a list of products available to them in their country.
Order information can be synchronised much more frequently, we recommend usage of the since parameter to restrict the volume of data being listed in each call the fetch Order information. The since parameter will filter the Orders endpoint to show only Orders that have been updated since a specified datetime. For instance, if Order information is synced every hour, then the since parameter should be used to request Orders updated in the last hour. An overlap can be used to ensure order updates are not missed.

There is also a pingback capability – to allow for near real-time Order updates. We recommend using a combination of both approaches to allow prompt updates and ensure nothing is missed.

## Suppliers

Suppliers will need to fetch order information from the API periodically (polling) and act accordingly. To act on an order one of the documented order actions will be required.

When an order is created, we can send an email notification. This could be used as a prompt to fetch new order information but we would recommend polling be carried out in parallel. If using email notifications for this purpose, an integration would have to take account of the various concurrent requests and actions.

## Function overview
Each of the main objects/entities available from GPS are introduced along with recommendations for how to use these in your ordering system.

### Categories
Categories are also provided as a service/endpoint, while these are reasonably stable, we do recommend syncing the categories you keep locally against this service every time you go to pull product information. The Products will refer to these categories. You can of course decide to categorise Products in your own way and ignore the Product Categories from GPS – we provide these Categories (as we use them ourselves) for your reference, should you require.

### Regions
We supply a list of all the countries rewards are available in by geographical region. The country name, ISO code and the id of this country as used by our system is available from the GPS api. It is possible that not every country available from GPS will have rewards in that you have been allocated on your API key.

### Catalogues
Catalogues are the primary dimension along which we organise our products. An API key is granted access to one or more catalogues according to the requirements of the Ordering system and their agreement with Corporate Rewards. Catalogue allocation is also quite stable but we recommend iterating over each Catalogue in order to fetch product information to ensure that you are always getting the correct information.

### Products
Once a list of Catalogues is obtained, a list of products should be fetched from this catalogue. Depending on the catalogue there may be hundreds of products so this call may take several seconds to complete. There are parameters provided to manipulate the information coming back from the API - see the Catalogues section later.

When getting Product information, we recommend checking each individual product in the feed (perhaps by hashing or somehow fingerprinting the actual product information) to see if it is new or has changed before adding it to the local store. This can speed up your processing of Products. It is possible that some products will have been removed from the feed, you will need to react to this situation and remove the product from your local store in this case.

#### Variants and Vouchers
If we take an example case for a product that is available in a variety of colours, this information is stored against a product as a series of one or more variants against a 'base product'. For instance, the Apple iPhone is available by default in black as the base product but can also be ordered in variants of gold or rose gold - therefore there are two elements under the variants key for this product. These variants are chiefly differentiated by their variant sku and the description of the reason for the variation. This means that we can have one product with a total of three variants as opposed to 3 products - there is one restriction, namely that the price of all these variants must be the same. When ordering you will need to provide the product_id and the sku of either the product or the chosen variant.

When vouchers were added to the product catalogue, it was decided to use the product variant capability to store denominations for a voucher rather than add a new product for every denomination of a voucher. This does break the rule that all product variants must cost the same, so this lead to the introduction of the voucher flag - a boolean attribute on the product. This is used to indicate that this product is a set of voucher denominations (as opposed to a merchandise type product). The way voucher type products are configured is as follows:
*   The lowest denomination voucher details are used to create the product, this also creates a single variant (denomination)
*   Subsequent denominations are also added as variants
*   The description of the voucher denomination is always recorded as the variant attribute (i.e. $500) and is suitable for displaying to users as a choice
*   To reflect the GBP pricing expected of regular products and to allow a similar price calculation by ordering systems, each denomination has face_value_gbp and variant_base_price attributes which are calculated in GBP with any relevant/agreed markup in place

An ordering system should calculate the cost of a voucher denomination using the variant_base_price attribute

#### Price calculation
Products will be available in a set of one or more Countries however the base_price cost of an Product will always be presented in Pounds Sterling to an ordering system. To calculate the cost of an Order to an ordering system, the delivery Country will need to be known. The cost of the Product plus tax is calculated using the base_price field value then adding the sales tax using the Products vat_rate field value. This subtotal needs to be added to the delivery cost for the destination country. To calculate the delivery cost, use the delivery_charge value from the destination country your Order is being delivered to and add in the vat_rate for the delivery charge. This yields:

Product (base_price + vat_rate) + (available_countries{delivery_charge + vat_rate})

### Orders
Once your local store of Regions, Catalogues, Categories and Products are up to date, products can be displayed to users for them to order. Ordering systems have to decide if a user is eligible to order a product (has enough points or is over the minimum age) and pass the order information to GPS, if the order can be placed, GPS will respond with the Order information.

An order can only ever be created for one product, referenced primarily by its id. In order to facilitate more flexible ordering of vouchers and prepaid cards, GPS requires orders to be created with one or more line items.

An order for a physical product should have one line item describing the product, it’s variant (sku) and quantity required.
When ordering a voucher or prepaid card, it is possible to order a combination of voucher/top up denominations. For instance, a user may wish to order a certain amount of points worth of credit on a prepaid card such as a £10, £20 and 3 multiples of a £50 credit/top up totalling £180, rather than processing 3 separate orders, a user can (and should be provided with the relevant user interface to do so) place one order for all the required amounts.

In the case of ordering multiple denominations of a voucher product, the product_id is required as an order attribute and each combination of sku (for the particular denomination) and quantity are supplied in the line_item attributes.

#### Calculating the cost of an order

In order to calculate to total cost of an order, the follwing calculations apply, given that:

p = product.base_price * (1 + prooduct.vat_rate)

q = line_item.quantity

c = product.available_countries\[order.country\]

d = c.delivery_cost * (1 + c.vat_rate)

Then the gross cost of an order for an order of a physical product is:

(p + d) * q

However for voucher products (indicated by product.is_voucher = true) the calculation is different. A voucher typically has variants which are used as denominations and as such, each denomination will have it's own base_price (as opposed to the base_price against the product). Orders for vouchers can have multiple line_items, each for a different denomination. To calculate the gross cost of an order for one or more vouchers:

For each line_item, given that:

v = product.variants\[order.line_items.line_item.product_sku\]

p = v.variant_base_price * (1 + product.vat_rate)

q = line_item.quantity

s = p * q

then:

t = the sum of s for each line_item

c = product.available_countries\[order.country\]

d = c.delivery_cost * (1 + c.vat_rate)

So the grand total with the deliverycost is:

t + d

An order for one or more vouchers is only charged one delivery_cost in total

Ordering systems can then charge whatever mark-up on top of the calculated cost of the order they choose as well as converting to whatever local ‘currency’ is used to present orders to the end user. Many ordering systems use ‘Points’ to obscure the true value/cost of a Product. A ratio of points/Pound is typically chosen by the ordering system.

We strongly suggest coupling the charge to an end user with the total cost of the order.

#### Completed orders

Once and order is complete, the Ordering system should store order information (returned from a successful placement of an order) locally together with information to identify which user this Order information was for. Orders are created with a state (status) of Pending. When a supplier accepts an Order it moves to Processing, when dispatched by a supplier the Order state is set to Dispatched and tracking information is provided when available. If a pingback URL is supplied (when creating the order) that uniquely identifies this order on your system, whenever a change is made to the order in GPS a simple GET request will be made to this URL. This should signal the ordering system to fetch the updated information for this order. This is optional but will provide a near-realtime update for an order including tracking information.

It is possible to request a list of orders made by your programme/api key and it is possible to filter the order using the parameters detailed in the section pursuant to orders below.

We recommend that ordering systems periodically sync order information with GPS to track changes in order status and show this information to end users. Relying soley on the pingback URL mechanism is not recommended as it may result in stale information on the ordering side if the pingback is not successful - a ping back is only sent once

# Categories

## List categories

This endpoint fetches a JSON array of categories. There is a simple parent relationship represented using the parent_id field. This enables an ordering system that chooses to use the Categories from GPS to re-create the category structure. Product data has a reference to the GPS Category they are in using the category_id field.

``` http
GET /api/v2/categories HTTP/1.1
Authorization: Token token=xxx
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id" : 1,
    "name" : "category name",
    "parent_id" : null
  },
  {
    "id" : 2,
    "name" : "category name",
    "parent_id" : 1
  }
  ...
]
```

### HTTP Request

`GET /api/v2/categories`

# Regions

## List all Regions and the Countries within them

This endpoint fetches a JSON array of Regions each with an array of countries in that region. Each region will show a list of the countries in that region as a nested list of objects.

``` http
GET /api/v2/regions HTTP/1.1
Authorization: Token token=xxx
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id" : 1,
    "name" : "Western Europe",
    "countries" : [
      {
        "id" : 1,
        "name" : "United Kingdom",
        "currency_id" : 2,
        "vat_rate_id" : 6,
        "iso_code" : "UK",
        "dialing_code": "+44",
        "currency": {
            "id": 1,
            "title": "Pounds Sterling",
            "abbreviation": "GBP",
            "sign": "£",
            "name": "Pounds Sterling",
            "exchange_rate": 1
        }
      },
      {
        "id" : 2,
        "name" : "France",
        "currency_id" : 6,
        "vat_rate_id" : 3,
        "iso_code" : "FR",
        ,
        "currency": {
            "id": 2,
            "title": "Euro",
            "abbreviation": "EUR",
            "sign": "€",
            "name": "Euro",
            "exchange_rate": 1.23
        }
      }
    ]
  },
  ...
]
```

### HTTP Request

`GET /api/v2/regions`

### Attributes

Attribute | Type | Info
--------- | ---- | ----
none | |

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

### HTTP Request

`GET /api/v2/catalogues`

### Attributes

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

### HTTP Request

`GET /api/v2/catalogues/1/products`

### Attributes

Attribute | Type | Info
--------- | ---- | ----
status | string | ordering systems will only see products with a status of approved - this is largely functionless for ordering systems
minimum_age | integer | when present only products suitable for a minimum age of this value will be shown - for instance a value of 18 will only list products that have either no minimum age or a value greater then or equal to 18
delivery_type | string | String (may be comma separated) delivery_types are typically Physical, Email, Downloadable and Prepaid. Products will only have this or these delivery type if this parameter is used
country | string | This should be a proper case, url encoded string of a Country name (as reported in the Regions endpoint). Only products available in this coutry will be listed

# Orders

## List all orders

This endpoint fetches a JSON array of orders you are allowed to see. Typically, ordering systems will want to periodically syncronise Orders so as to show up to date information to users about their Orders. By default this endpoint will fetch **all** orders from the first order ever placed by the client. This can grow to a long list over time and so becomes slow and taxing to fetch and process. We have provided some parameters for filtering this list of orders so clients can more easily stay up to date. We recommend using the since parameter - this will return a list of orders that have been updated on or after the date and time provided as this value. Typically ordes are updated daily but no more frequently than hourly.

``` http
GET /api/v2/orders HTTP/1.1
Authorization: Token token=xxx
```

``` http
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "id": 1142,
    "supplier_id": 50,
    "number": "9001056",
    "product_id": 5202,
    "ordered_at": "2016-12-09T16:53:26.000Z",
    "programme_id": 1,
    "customer_name": "Ted Test",
    "customer_phone": "7189404020",
    "customer_email": "ted.moyses@corporaterewards.co.uk",
    "recipient_address": "8 The Courtyard \nTimothy's Bridge Road \nStratford Upon Avon \nUK \nCV37 9NP",
    "delivery_instructions": null,
    "voucher_card_number": null,
    "tracking_id": null,
    "tracking_url": null,
    "dispatch_date": null,
    "created_at": "2016-12-09T16:53:26.000Z",
    "updated_at": "2016-12-09T16:53:26.000Z",
    "recipient_name": "Ted Test",
    "recipient_phone": "7189404020",
    "recipient_email": "ted.moyses@corporaterewards.co.uk",
    "state": "pending",
    "quantity": 1,
    "variant_id": 1000,
    "country_id": 140,
    "pingback_url": null,
    "po_number": null,
    "product_name": "Amazon.co.uk eVoucher",
    "product_sku": "AMA10|1",
    "product_variation": "Variant: 10",
    "delivery_type": "Email",
    "international_requirements": false,
    "price": "9.7",
    "delivery_cost": 0,
    "points": 1217,
    "product_base_price": null,
    "product_vat_rate": "8.0",
    "api_key_id": null,
    "remote_user_id": null,
    "voucher": true,
    "delivery_charge": 0,
    "delivery_vat_numeric": 0,
    "delivery_vat_name": "0.0%",
    "old_order_type": false,
    "customer_address_1": "8 The Courtyard",
    "customer_address_2": "Timothy's Bridge Road",
    "town": "Stratford Upon Avon",
    "postcode": "CV37 9NP",
    "country_name": "UK",
    "remote_order": "",
    "currency_id": 3,
    "transactions": [
      {
        "id": 1234,
        "order_id": 1142,
        "user_id": null,
        "variety": "Redemption",
        "points": 1217,
        "reason": "Initial points value of customer order",
        "transaction_type": "Debit"
      }
    ],
    "line_items": [
      {
        "id": 1121,
        "order_id": 1142,
        "name": "Amazon.co.uk eVoucher",
        "info": "Variant: 10",
        "sku": "AMA10|1",
        "product_id": 5202,
        "product_variant_id": 1000,
        "delivery_type": "Email",
        "price": 9.7,
        "quantity": 1,
        "points": null,
        "individual_price": 9.7
      }
    ],
    "comments": [
      {
        "id": 375,
        "user_id": 5,
        "order_id": 1142,
        "content": "Order Created",
        "created_at": "2016-12-09T16:53:26.000Z",
        "updated_at": "2016-12-09T16:53:26.000Z"
      }
    ],
    "programme": {
        "id": 123,
        "created_by_client": null,
        "name": "Client Programme",
        "reference": "1234",
        "created_at": "2017-09-27T08:29:27.000Z",
        "updated_at": "2017-09-27T08:29:27.000Z"
    }
  }
]
```
### HTTP Request

`GET /api/v2/orders`

### Parameters

Parameter | Type | Info
--------- | ---- | ----
status | String | Optional - only orders with this status will be returned. Must be one of: pending, processing, dispatched, cancelled, unable to fulfill, cancel requested, pending return
programme_ref | String | Optional - used to fetch orders for a given programme. Most ordering system api keys are only allocated/linked to one programme, ordering systems can only access orders for their own programme. This parameter has no effect unless your api key is linked to more than one programme. You should be issued your programme reference along with your api key
since | Datetime | Sending this parameter with a parsable datetime as the value will only return orders that have been updated since this datetime value. This is useful to poll for orders with new information. Without this parameter present, all orders since the start of your programme (that also match any other provided parameters) will be returned. Example 2017-11-21T23:56:34Z

## Creating orders

This endpoint allows ordering systems to place orders on GPS. In order to place an order you will need:
-   The id of a product and the SKU shall be provided in the line_item. If the product has variants, line_item will require the SKU of the variant. For vouchers, more than one line_item can be provided.
-   A country name - as listed in the /regions endpoint
-   Email address - used for delivery (i.e. for an eVoucher) and to notify users
-   A full address for a product with delivery_type = Physical

If the product is at status Approved and is available (product data can change since an ordering system last refreshed it's product data) at the time of order and the above information is provided correctly - GPS will respond with an HTTP status code of 200. Otherwise a status code of >= 400 will be returned as per REST expectations along with an explanatory plain text message as body content. Ordering systems are expected to track and respond to the response status codes.

The line_items object is a hash of array items and as such will require unique keys for each item. The value of these keys is not used.

```
POST /api/v2/orders HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
	"order": {
		"programme_ref": "1309",
		"product_id": "3257",
		"quantity": "1",
		"points": "100",
		"customer_name": "Test Order User",
		"customer_email": "email@address.com",
		"customer_phone": "447502375063",
		"customer_address_1": "22 A street",
		"customer_address_2": "",
		"country": "UK",
		"town": "renfrew",
		"postcode": "pa4 8qy",
		"recipient_address": "22 A street \n\nrenfrew \npa4 8qy",
		"recipient_name": "Mohammed Ashraf",
		"recipient_email": "",
		"recipient_phone": "447502375063",
		"delivery_instructions": ""
	},
	"line_items": {
		"0": {
			"line_item": {
				"name": "Sony Playstation 4 Console",
				"sku": "PS222",
				"quantity": "1",
				"points": "27720"
			}
		}
	}
}
```

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1246,
  "supplier_id": 3,
  "number": 9001154,
  "product_id": 3257,
  "ordered_at": "2017-01-30T17:23:25.945Z",
  "programme_id": 34,
  "customer_name": "Test Order User",
  "customer_phone": "447502375063",
  "customer_email": "email@address.com",
  "recipient_address": "22 A street \n \nrenfrew \nUK \npa4 8qy",
  "delivery_instructions": "",
  "voucher_card_number": null,
  "tracking_id": null,
  "tracking_url": null,
  "dispatch_date": null,
  "created_at": "2017-01-30T17:23:26.103Z",
  "updated_at": "2017-01-30T17:23:26.103Z",
  "recipient_name": "Mohammed Ashraf",
  "recipient_phone": "447502375063",
  "recipient_email": "",
  "state": "pending",
  "quantity": 1,
  "variant_id": null,
  "country_id": 140,
  "pingback_url": null,
  "po_number": null,
  "product_name": "Sony Playstation 4 Console",
  "product_sku": "PS222",
  "product_variation": null,
  "delivery_type": "Physical",
  "international_requirements": false,
  "price": "290.04",
  "delivery_cost": 7,
  "points": null,
  "product_base_price": "290.04",
  "product_vat_rate": "20.0",
  "api_key_id": 12,
  "remote_user_id": null,
  "voucher": false,
  "delivery_charge": 6.5,
  "delivery_vat_numeric": 20,
  "delivery_vat_name": "20.0%",
  "old_order_type": false,
  "customer_address_1": "22 A street",
  "customer_address_2": "",
  "town": "renfrew",
  "postcode": "pa4 8qy",
  "country_name": "UK",
  "remote_order": null,
  "currency_id": 3,
  "transactions": [
    {
      "id": null,
      "order_id": 1246,
      "user_id": null,
      "variety": "Redemption",
      "points": null,
      "reason": "Initial points value of customer order",
      "transaction_type": "Debit"
    }
  ],
  "line_items": [
    {
      "id": 1232,
      "order_id": 1246,
      "name": "Sony Playstation 4 Console",
      "info": null,
      "sku": "PS222",
      "product_id": null,
      "product_variant_id": null,
      "delivery_type": "Physical",
      "price": 290.04,
      "quantity": 1,
      "points": 27720,
      "individual_price": 290.04
    }
  ],
  "comments": []
}
```

### HTTP Request

`POST /api/v2/orders`

### Attributes

Attribute | Type | Info
--------- | ---- | ----
order\[programme_ref\] | String | Optional - Please provide the programme reference issued to you by Corporate Rewards - if you do not know this value, it will be inserted automatically
order\[product_id\] | Integer | Required - this is the product.id you get from the product in the catalogues endpoint
order\[quantity\] | Integer | Required - this is required to be calculated from the sum of all quantities in all line items.
order\[points\] | Integer | Optional - the total amount of points the user was charged by the ordering system for this product including delivery. GPS does not care about points per se but this can be useful when Rewards services are talking about an order as part of a customer service query. This should reflect the total points for all line items in an order. Using this value will reflect a transaction in the transactions key in the returned order data.
order\[customer_name\] | String | Required - This is the name of the customer who placed the order
order\[customer_email\] | String | Required - This is the email address of the customer who placed the order
order\[customer_phone\] | String | Required - this is the phone number of the customer who placed the order
order\[customer_address_1\] | String | Required - this is the first line of the address, required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products
order\[customer_address_2\] | String | Optional - second line of address
order\[country\] | String | Required - this is the name of the country this product will be delivered to. Countries are available from the /regions endpoint or from the list of available_countries for this product. Always use the name of the country (not the id). Required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products
order\[town\] | String | Required - this is the postal town this product will be delivered to. Required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products
order\[postcode\] | String | Postal (or zip) code this product is to be delivered to. Required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products
order\[recipient_address\] | String | Required - this is the concatenated version of the full postal address this product will be delivered to. Required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products
order\[recipient_name\] | String | Optional - This allows a user to place an order and have it delivered to another person i.e. as a gift.
order\[recipient_email\] | String | Optional - This allows a user to sepcify an alternate email address to be the recipient of this order
order\[recipient_phone\] | String | Optional - string representation of an alternate phone number to be provided as the recipient of this product
order\[delivery_instructions\] | String | Optional - allows for the provision of extra instructions for the delivery of this order such as "Please knock, the door bell is not working"
order\[remote_order\] | String | Optional - this can be used to record the ordering systems own unique id for this order. Some systems will create an order locally before attempting to push the order over the api to GPS. This field can be used to store this id value for later cross referencing.
order\[pingback_url\] | String | Optional - if provided, must be a valid url (with scheme i.e. http:// or https://) that identifies this order locally on the ordering system. If an order is updated by GPS, we will make a simple get request to this url. This is a signal that an update has occured and the ordering system should fetch the order by id (GPS order.id). The URL provided should not require any kind of authentication and as such should also not 'leak' any information.
line_items\[0\]\[line_item\]\[name\] | String | Required - this is the name of the product being ordered
line_items\[0\]\[line_item\]\[sku\] | String | Required - this is the SKU of the product being order. If ordering a variant of a product, ALWAYS provide the variant SKU
line_items\[0\]\[line_item\]\[quantity\] | Integer | Required - the number of this product to devliver
line_items\[0\]\[line_item\]\[points\] | Integer | Optional - the value of points charged for this line item. This is used to indicate to GPS how many points each line item is work but is not related to the total points for the order. This has no effect in GPS other than to indicate points for a line item at time of order.

### Error messages and response codes

StatusCode | Likely cause
---------- | ------------
404 | This product either does not exist or is no longer available
403 | API key supplied does not have permission to use this endpoint
422 | The order information supplied is invalid - there will be an accompanying message in the body of the response to explain more about why the request is un acceptable
500 | Something unexpected has gone wrong on the server side - there may be a detailed error message from the staging environment, please copy and paste any such error to either the Product & Proposition or Technical Team at CR to assist with diagnosing this error

Possible reasons for a 422:

- `{{ProductName}}` can't be delivered to this country
- `{{ProductName}}` isn't available to order
- `{{order[pingback_url]}}` isn't a valid URL
- `{{order[product_id]}}` is not supplied by Supplier: `{{SupplierName}}`
- can't be blank or is an unknown supplier.
- can't be blank or is an unrecognised country.
- can't be blank or is an unrecognised product.
- can't be blank or is an unrecognised programme.
- One or more line items are invalid. It is likely that the corresponding
  voucher, variant or product may have been deleted in GPS - {{addtional line items errors may appear here, if known}}
- One or more line items are invalid - {{more line item related messages may be displayed here, if known}}
- No Line Items found
- `{{line_items[0][line_item][name]}}` SKU does not match order product SKU

# Order actions

Once you have list of relevant orders to work on, you can perform a limit number of actions on these orders to progress these orders along the order management lifecycle. The order status transitions are governed by a state machine – this state machine will only allow orders to transition between states that valid starting and ending states for a given action. For instance a 'pending' order may transition to 'processing' but not to 'dispatched'.
Any attempt to update an order using an invalid state transistion will be met with a 422 (unprocessable entity and a textual description of the error)
In principle the vast majority of actions a supplier will be performing are:

## accept

This action requires an order ID (in this case 123) with a current status of 'pending' resulting in the order having a new status of
'processing'

```
POST /api/v2/orders/123/accept HTTP/1.1
Authorization: Token token=xxx
```

```
HTTP/1.1 200 OK
Content-Type: application/json


```

Attribute | Type | info
--------- | ---- | ----
None

## reject

This action requires an order ID with a current status of 'pending'  and will result in the order having the status 'unable to fulfill'. A reason is required for any order being rejected.

```
POST /api/v2/orders/123/reject
Authorization: Token token=xxx

{
  "order": {
    "reason": "Reason for supplier rejecting this order"
  }
}
```

```
HTTP/1.1 200 OK
Content-Type: application/json


```

Attribute | Type | Info
--------- | ---- | ----
order\[reason\] | String | Required - text description of why the order cannot be fulfilled and is being rejected. Care should be taken when providing a reason as this will be seen by the end user/customer as well as our reward services team.

## dispatch

This action requries an order ID with a current status of 'processing' and will result in this order having the status 'dispatched'. In order to despatch an order you will have to provide a date at which the order was despatched and optionally either a tracking url or tracking id or (prefereably) both if required to trace the delivery using a courier.

```
POST /api/v2/orders/123/dispatch
Authorization: Token token=xxx

{
  "order": {
    "dispatch_date": "2018-01-09 12:34:56",
    "tracking_id": "",
    "tracking_url": "https://tracking.com/123456"
  }
}
```

```
HTTP/1.1 200 OK
Content-Type: application/json


```

Attribute | Type | Info
--------- | ---- | ----
order\[dispatch_date\] | DateTime (string) | Required - string parsable as datetime (i.e. 2017-01-01 13:45:56) this should be the date and time the order was despatched at - there are currently no restrictions on whether this date is in the past or present, however logically it should not be before the order was accepted for processing
order\[tracking_id\] | String | Optional - this value should represent an identifier or token for tracking the delivery with the relevant courier or handler
order\[tracking_url\] | String | Optional - if provided, must be a valid url (i.e. https://courier.com/track?parcelID=232456) and should allow the user to click and be shown the progress of their delivery.

