---
title: GPS - API Reference

language_tabs:

includes:
- authentication
- process_flow
- translations
- api/categories
- api/regions
- api/catalogues
- api/orders
- api/order_actions
- api/currencies/_0_currencies
- api/currencies/list_all_currencies
- api/products/_0_products
- api/products/list_all_products
- api/products/create_product_v2
- api/products/update_product
- api/products/delete_product
- api/product_actions

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
| GET [/api/v2/categories](#orders-list-all-orders) | Orderer |
| GET [/api/v2/regions](#regions)  | Orderer  |
| GET [/api/v2/catalogues](#catalogues)  | Orderer  |
| GET [/api/v2/catalogues/{catalogue_id}/products](#catalogues-list-all-products-in-a-catalogue) | Orderer  |
| GET [/api/v2/orders](#orders)  | Orderer & Supplier |
| POST [/api/v2/orders](#orders-creating-orders)  | Orderer |
| POST [/api/v2/orders/{order_id}/accept](#order-actions-accept) | Supplier |
| POST [/api/v2/orders/{order_id}/reject](#order-actions-reject) | Supplier |
| POST [/api/v2/orders/{order_id}/dispatch](#order-actions-dispatch) | Supplier |
| GET [api/v2/products](#products-list-products) | Supplier |
| POST [api/v2/products](#products-create-product) | Supplier |
| PATCH [api/v2/products/{product_id}](#products-update-product) | Supplier |
| PUT [api/v2/products/{product_id}](#products-update-product) | Supplier |
| DELETE [api/v2/products/{product_id}](#products-delete-product) | Supplier |

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
