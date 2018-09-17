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

#### Categories
Categories are also provided as a service/endpoint, while these are reasonably stable, we do recommend syncing the categories you keep locally against this service every time you go to pull product information. The Products will refer to these categories. You can of course decide to categorise Products in your own way and ignore the Product Categories from GPS – we provide these Categories (as we use them ourselves) for your reference, should you require.

#### Regions
We supply a list of all the countries rewards are available in by geographical region. The country name, ISO code and the id of this country as used by our system is available from the GPS api. It is possible that not every country available from GPS will have rewards in that you have been allocated on your API key.

#### Catalogues
Catalogues are the primary dimension along which we organise our products. An API key is granted access to one or more catalogues according to the requirements of the Ordering system and their agreement with Corporate Rewards. Catalogue allocation is also quite stable but we recommend iterating over each Catalogue in order to fetch product information to ensure that you are always getting the correct information.

#### Products
Once a list of Catalogues is obtained, a list of products should be fetched from this catalogue. Depending on the catalogue there may be hundreds of products so this call may take several seconds to complete. There are parameters provided to manipulate the information coming back from the API - see the Catalogues section later.

When getting Product information, we recommend checking each individual product in the feed (perhaps by hashing or somehow fingerprinting the actual product information) to see if it is new or has changed before adding it to the local store. This can speed up your processing of Products. It is possible that some products will have been removed from the feed, you will need to react to this situation and remove the product from your local store in this case.

##### Variants and Vouchers
If we take an example case for a product that is available in a variety of colours, this information is stored against a product as a series of one or more variants against a 'base product'. For instance, the Apple iPhone is available by default in black as the base product but can also be ordered in variants of gold or rose gold - therefore there are two elements under the variants key for this product. These variants are chiefly differentiated by their variant sku and the description of the reason for the variation. This means that we can have one product with a total of three variants as opposed to 3 products - there is one restriction, namely that the price of all these variants must be the same. When ordering you will need to provide the product_id and the sku of either the product or the chosen variant.

When vouchers were added to the product catalogue, it was decided to use the product variant capability to store denominations for a voucher rather than add a new product for every denomination of a voucher. This does break the rule that all product variants must cost the same, so this lead to the introduction of the voucher flag - a boolean attribute on the product. This is used to indicate that this product is a set of voucher denominations (as opposed to a merchandise type product). The way voucher type products are configured is as follows:
*   The lowest denomination voucher details are used to create the product, this also creates a single variant (denomination)
*   Subsequent denominations are also added as variants
*   The description of the voucher denomination is always recorded as the variant attribute (i.e. $500) and is suitable for displaying to users as a choice
*   To reflect the GBP pricing expected of regular products and to allow a similar price calculation by ordering systems, each denomination has face_value_gbp and variant_base_price attributes which are calculated in GBP with any relevant/agreed markup in place

An ordering system should calculate the cost of a voucher denomination using the variant_base_price attribute

##### Price calculation
Products will be available in a set of one or more Countries however the base_price cost of an Product will always be presented in Pounds Sterling to an ordering system. To calculate the cost of an Order to an ordering system, the delivery Country will need to be known. The cost of the Product plus tax is calculated using the base_price field value then adding the sales tax using the Products vat_rate field value. This subtotal needs to be added to the delivery cost for the destination country. To calculate the delivery cost, use the delivery_charge value from the destination country your Order is being delivered to and add in the vat_rate for the delivery charge. This yields:

Product (base_price + vat_rate) + (available_countries{delivery_charge + vat_rate})

#### Orders
Once your local store of Regions, Catalogues, Categories and Products are up to date, products can be displayed to users for them to order. Ordering systems have to decide if a user is eligible to order a product (has enough points or is over the minimum age) and pass the order information to GPS, if the order can be placed, GPS will respond with the Order information.

An order can only ever be created for one product, referenced primarily by its id. In order to facilitate more flexible ordering of vouchers and prepaid cards, GPS requires orders to be created with one or more line items.

An order for a physical product should have one line item describing the product, it’s variant (sku) and quantity required.
When ordering a voucher or prepaid card, it is possible to order a combination of voucher/top up denominations. For instance, a user may wish to order a certain amount of points worth of credit on a prepaid card such as a £10, £20 and 3 multiples of a £50 credit/top up totalling £180, rather than processing 3 separate orders, a user can (and should be provided with the relevant user interface to do so) place one order for all the required amounts.

In the case of ordering multiple denominations of a voucher product, the product_id is required as an order attribute and each combination of sku (for the particular denomination) and quantity are supplied in the line_item attributes.

##### Calculating the cost of an order

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

##### Completed orders

Once and order is complete, the Ordering system should store order information (returned from a successful placement of an order) locally together with information to identify which user this Order information was for. Orders are created with a state (status) of Pending. When a supplier accepts an Order it moves to Processing, when dispatched by a supplier the Order state is set to Dispatched and tracking information is provided when available. If a pingback URL is supplied (when creating the order) that uniquely identifies this order on your system, whenever a change is made to the order in GPS a simple GET request will be made to this URL. This should signal the ordering system to fetch the updated information for this order. This is optional but will provide a near-realtime update for an order including tracking information.

It is possible to request a list of orders made by your programme/api key and it is possible to filter the order using the parameters detailed in the section pursuant to orders below.

We recommend that ordering systems periodically sync order information with GPS to track changes in order status and show this information to end users. Relying soley on the pingback URL mechanism is not recommended as it may result in stale information on the ordering side if the pingback is not successful - a ping back is only sent once


