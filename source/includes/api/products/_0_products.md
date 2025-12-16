# Products

Access to products endpoint will be scoped to your supplier in GPS.

**Important note**

All products and product variants are assigned an id upon creation. It is important to note that the id assigned to product variants is subject to change regularly due to how updates are managed, and any product that is deleted and recreated will also receive a new id; therefore, it is vital that the SKU of a product or variant is considered as the unique identifier when evaluating products and their variants.

When you update or create a product via the API, the product id and the ids of all of the product's variants _(if it has any)_ will be returned. If you require the current variant ids then you can get them from the product response, or you can list all of your products and variants using our [list products endpoint](#products-list-all-products).
