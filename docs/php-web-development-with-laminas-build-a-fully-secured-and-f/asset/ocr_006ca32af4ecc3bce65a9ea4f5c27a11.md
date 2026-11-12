```
```php
$this->price = ($data['price'] ?? 0.0);
$this->discount = new Discount();
$this->discount->code = ($data['code_discount'] ??
0);
$this->discount->name = ($data['name_discount'] ??
'');
}
```
```

Observe that we have added a name_discount key.

Then, let us change the Product->toArray method to this content:

```
```php
public function toArray()
{
    $attributes = get_object_vars($this);
    unset($attributes['discount']);
    unset($attributes['name_discount']);
    $attributes['code_discount'] = $this->
    discount->code;

    return $attributes;
}
```
```

Observe that we removed the name_discount key because this field is not persisted in the products table.

Where does this name_discount key come from? This key will be recovered by the ProductTable class, but we must override the getAll method in this class by making an INNER JOIN SQL statement. The ProductTable->getAll method must be as follows:

```
```php
public function getAll($where = null): iterable {
    $select = new Select($this->tableGateway->
    getTable());

    $select->join('discounts', 'products.code_Discount=
    discounts.code', ['name_discount' => 'name']);

    if (!is_null($where)) {
        $parsedWhere = [];
        foreach($where as $index => $value) {
            $parsedWhere['products.' . $index] =
            $value;
        }
    }
```
```