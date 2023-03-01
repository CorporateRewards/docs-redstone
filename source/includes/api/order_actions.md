# Order actions

Once you have list of relevant orders to work on, you can perform a limit number of actions on these orders to progress these orders along the order management lifecycle. The order status transitions are governed by a state machine – this state machine will only allow orders to transition between states that valid starting and ending states for a given action. For instance a 'pending' order may transition to 'processing' but not to 'dispatched'.
Any attempt to update an order using an invalid state transistion will be met with a 422 (unprocessable entity and a textual description of the error)
In principle the vast majority of actions a supplier will be performing are:

## accept

This action requires an order ID (in this case 123) with a current status of 'pending' resulting in the order having a new status of
'processing'

> Request

``` http
POST /api/v2/orders/{id}/accept HTTP/1.1
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json


```

Attribute | Type | info
--------- | ---- | ----
None

## reject

This action requires an order ID with a current status of 'pending'  and will result in the order having the status 'unable to fulfill'. A reason is required for any order being rejected.

> Request

``` http
POST /api/v2/orders/{id}/reject HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
  "order": {
    "reason": "Reason for supplier rejecting this order"
  }
}
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```

Attribute | Type | Info
--------- | ---- | ----
order\[reason\] | String | Required - text description of why the order cannot be fulfilled and is being rejected. Care should be taken when providing a reason as this will be seen by the end user/customer as well as our reward services team.

## dispatch

This action requries an order ID with a current status of 'processing' and will result in this order having the status 'dispatched'. In order to despatch an order you will have to provide a date at which the order was despatched and optionally either a tracking url or tracking id or (prefereably) both if required to trace the delivery using a courier.

> Request

``` http
POST /api/v2/orders/{id}/dispatch HTTP/1.1
Authorization: Token token=xxx
Content-Type: application/json

{
  "order": {
    "dispatch_date": "2018-01-09 12:34:56",
    "tracking_id": "",
    "tracking_url": "https://tracking.com/123456"
  }
}
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```

Attribute | Type | Info
--------- | ---- | ----
order\[dispatch_date\] | DateTime (string) | Required - string parsable as datetime (i.e. 2017-01-01 13:45:56) this should be the date and time the order was despatched at - there are currently no restrictions on whether this date is in the past or present, however logically it should not be before the order was accepted for processing
order\[tracking_id\] | String | Optional - this value should represent an identifier or token for tracking the delivery with the relevant courier or handler
order\[tracking_url\] | String | Optional - if provided, must be a valid url (i.e. https://courier.com/track?parcelID=232456) and should allow the user to click and be shown the progress of their delivery.

