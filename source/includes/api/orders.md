# Orders

## List all orders

This endpoint fetches a JSON array of orders you are allowed to see. Typically, ordering systems will want to periodically syncronise Orders so as to show up to date information to users about their Orders. By default this endpoint will fetch **all** orders from the first order ever placed by the client. This can grow to a long list over time and so becomes slow and taxing to fetch and process. We have provided some parameters for filtering this list of orders so clients can more easily stay up to date. We recommend using the since parameter - this will return a list of orders that have been updated on or after the date and time provided as this value. Typically ordes are updated daily but no more frequently than hourly.

> Request

``` http
GET /api/v2/orders HTTP/1.1
Authorization: Token token=xxx
```

> Response

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
    "variant_id": 1000,
    "country_id": 140,
    "pingback_url": null,
    "po_number": null,
    "product_name": "Amazon.co.uk eVoucher",
    "product_sku": "A-123",
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
        "sku": "A-123",
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
#### HTTP Request

`GET /api/v2/orders`

#### Parameters

Parameter | Type | Info
--------- | ---- | ----
status | String | Optional - only orders with this status will be returned. Must be one of: pending, processing, dispatched, cancelled, unable to fulfill, cancel requested, pending return
programme_ref | String | Optional - used to fetch orders for a given programme. Most ordering system api keys are only allocated/linked to one programme, ordering systems can only access orders for their own programme. This parameter has no effect unless your api key is linked to more than one programme. You should be issued your programme reference along with your api key
since | Datetime | Sending this parameter with a parsable datetime as the value will only return orders that have been updated since this datetime value. This is useful to poll for orders with new information. Without this parameter present, all orders since the start of your programme (that also match any other provided parameters) will be returned. Example 2017-11-21T23:56:34Z

## Creating orders

This endpoint allows ordering systems to place orders on GPS. In order to place an order you will need:

- The id of a product and the SKU shall be provided in the line_item. If the product has variants, line_item will require the SKU of the variant. For vouchers, more than one line_item can be provided.

- A country name - as listed in the /regions endpoint

- Recipient email address - required for a product where the product `delivery_type` = `email` and is the email address where the order will be sent.

- Recipient delivery address - required for a product where the product `delivery_type` = `physical` and is the delivery address where the order will be sent.

If the product is at status Approved and is available (product data can change since an ordering system last refreshed it's product data) at the time of order and the above information is provided correctly - GPS will respond with an HTTP status code of 200. Otherwise a status code of >= 400 will be returned as per REST expectations along with an explanatory plain text message as body content. Ordering systems are expected to track and respond to the response status codes.

The line_items object is a hash of array items and as such will require unique keys for each item. The value of these keys is not used.

> Request

``` http
POST /api/v2/orders HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
	"order": {
		"programme_ref": "1309",
		"product_id": "3257",
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
				"sku": "L-123",
				"quantity": "1",
				"points": "27720"
			}
		}
	}
}
```

> Response

``` http
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
  "variant_id": null,
  "country_id": 140,
  "pingback_url": null,
  "po_number": null,
  "product_name": "Sony Playstation 4 Console",
  "product_sku": "L-123",
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
      "sku": "L-123",
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

#### HTTP Request

`POST /api/v2/orders`

#### Attributes

Attribute | Type | Info
--------- | ---- | ----
order\[programme_ref\] | String | Optional - Please provide the programme reference issued to you by Corporate Rewards - if you do not know this value, it will be inserted automatically
order\[product_id\] | Integer | Required - this is the product.id you get from the product in the catalogues endpoint
order\[points\] | Integer | Optional - the total amount of points the user was charged by the ordering system for this product including delivery. GPS does not care about points per se but this can be useful when Rewards services are talking about an order as part of a customer service query. This should reflect the total points for all line items in an order. Using this value will reflect a transaction in the transactions key in the returned order data.
order\[customer_name\] | String | Required - This is the name of the customer who placed the order
order\[customer_email\] | String | Optional - This is the email address of the customer who placed the order
order\[customer_phone\] | String | Required - this is the phone number of the customer who placed the order
order\[customer_address_1\] | String | Required - this is the first line of the address, required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products
order\[customer_address_2\] | String | Optional - second line of address
order\[country\] | String | Required - this is the name of the country this product will be delivered to. Countries are available from the /regions endpoint or from the list of available_countries for this product. Always use the name of the country (not the id). Required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products
order\[town\] | String | Required - this is the postal town this product will be delivered to. Required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products
order\[postcode\] | String | Postal (or zip) code this product is to be delivered to. Required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products
order\[recipient_address\] | String | Required - this is the concatenated version of the full postal address this product will be delivered to. Required if this order is for a product with a delivery_type with requires_address = true (1) i.e. physically delivered products. If the delivery address has a Company Name, please include this at the start of the recipient address
order\[recipient_name\] | String | Optional - This allows a user to place an order and have it delivered to another person i.e. as a gift.
order\[recipient_email\] | String | Required for a product where the product `delivery_type` =  `email` - This allows a user to specify an alternate email address to be the recipient of this order and is used to receive products with a `delivery_type` = `email`
order\[recipient_phone\] | String | Optional - string representation of an alternate phone number to be provided as the recipient of this product
order\[delivery_instructions\] | String | Optional - allows for the provision of extra instructions for the delivery of this order such as "Please knock, the door bell is not working"
order\[remote_order\] | String | Optional - this can be used to record the ordering systems own unique id for this order. Some systems will create an order locally before attempting to push the order over the api to GPS. This field can be used to store this id value for later cross referencing.
order\[pingback_url\] | String | Optional - if provided, must be a valid url (with scheme i.e. http:// or https://) that identifies this order locally on the ordering system. If an order is updated by GPS, we will make a simple get request to this url. This is a signal that an update has occured and the ordering system should fetch the order by id (GPS order.id). The URL provided should not require any kind of authentication and as such should also not 'leak' any information.
line_items\[0\]\[line_item\]\[name\] | String | Required - this is the name of the product being ordered
line_items\[0\]\[line_item\]\[sku\] | String | Required - this is the SKU of the product being order. If ordering a variant of a product, ALWAYS provide the variant SKU
line_items\[0\]\[line_item\]\[quantity\] | Integer | Required - the number of this product to deliver
line_items\[0\]\[line_item\]\[points\] | Integer | Optional - the value of points charged for this line item. This is used to indicate to GPS how many points each line item is work but is not related to the total points for the order. This has no effect in GPS other than to indicate points for a line item at time of order.

#### Error messages and response codes

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


