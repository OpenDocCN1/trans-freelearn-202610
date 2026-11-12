+   - Checks whether the called controller name was `ProductAPIController`

+   - Checks whether the route name was `inventoryapi`

After implementing the test for the product updates, we will implement the test for product deletion.

# # Testing product deletion

Finally, to finish the API test, we will implement the `testProductionDelete` method in the `ProductAPIControllerTest` class. The code should look as follows:

```
public function testProductDelete() : void
{
    $productTable = $this->getApplication() ->
        getServiceManager()->get('ProductTable');
    $product = $productTable->getByField('name',
        'Infinity Gauntlet');
    $this->dispatch('/inventoryapi/productapi/' .
        $product->code, 'DELETE');
    $this->assertResponseStatusCode(200);
    $this->assertModuleName('inventory');
    $this->assertControllerName('productapi'); // as
        specified in router's controller name alias
    $this->assertControllerClass
        ('ProductAPIController');
    $this->assertMatchedRouteName('inventoryapi');
    $discountTable = $this->getApplication() ->
        getServiceManager()->get('DiscountTable');
    $discount = $discountTable->getByField('name', 'no
        discount');
    $discountTable->delete($discount->code);
}
```

The `testProductDelete` method does the following:

+   - Gets an instance of `ProductTable`

+   - Recovers the product inserted before

+   - Dispatches an HTTP DELETE request for the `/inventoryapi/productapi` endpoint (observe that argument to delete is the product code)