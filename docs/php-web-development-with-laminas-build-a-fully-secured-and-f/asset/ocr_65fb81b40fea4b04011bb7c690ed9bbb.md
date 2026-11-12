The constructor method receives an instance of ProductTable and assigns it to the attribute productTable:

```
public function __construct(ProductTable $productTable)
{
    $this->productTable = $productTable;
}
```

HTTP POST is mapped to the create method:

```
public function create($data)
{
    $product = new Product();
    $product->exchangeArray($data);
    $inserted = $this->productTable->save($product);
    return new JsonModel(['inserted' => $inserted]);
}
```

The $data argument, received by the create method, contains the data from $_POST.

HTTP GET (with an argument) is mapped to the get method:

```
public function get($id)
{
    $field = (is_numeric($id) ? 'code' : 'name');
    $id = (is_numeric($id) ? (int) $id : $id);
    $product = $this->productTable->getByField($field,
        $id);
    return new JsonModel(['product' => $product->
        toArray()]);
}
```

HTTP PUT is mapped to the update method:

```
public function update($id, $data)
{
    $product = $this->productTable->getByField
        ('code', $id);
    $product->exchangeArray($data);
    $updated = $this->productTable->save($product);
```