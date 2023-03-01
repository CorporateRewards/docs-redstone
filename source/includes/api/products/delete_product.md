## Delete Product

You can also delete products over the api as shown. Products can only be deleted by the supplier of the product. 
 
 Deleting a product will:
 
 - Remove the product as well as any variations and translations
 - Remove any associated media, providing it is not used by other products
 - Preserve the order history
 
 > Request

``` http
DELETE /api/v2/products/{product_id}
Authorization: Token token=xxx
```

> Response

``` http
HTTP/1.1 200 OK
Content-Type: application/json
```
