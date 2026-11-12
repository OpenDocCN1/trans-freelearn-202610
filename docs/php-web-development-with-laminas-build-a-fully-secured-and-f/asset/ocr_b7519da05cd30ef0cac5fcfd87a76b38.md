7. The getByField method, however, can be implemented by the AbstractTable class because it is generic:

```
public function getByField(string $field, $value) :
AbstractModel
{
	$where = [
		$field => $value
	];
	$rowSet = $this->tableGateway->select($where);
	if ($rowSet->count() == 0) {
		$modelName = $this->modelName;
		return new $modelName();
	}
	return $rowSet->current();
}
```

The getByField class allows searching for any field. As you have noticed, if no record is found, the method returns an instance of a class defined by the $this->modelName attribute. In the SchoolClassTable class, this attribute is declared as follows:

```
protected string $modelName = 'School\\SchoolClass';
```

This attribute allows each child class of AbstractTable to inform its model class and ensures that the method will return an instance of AbstractModel.

Wait! What is AbstractModel? It's a generalization for our models. It implements two generic methods, one for data input, exchangeArray, and the other for data output, toArray.

This is the model:

# AbstractModel.php

```
<?php
declare(strict_types = 1);
namespace School;

abstract class AbstractModel
{

	public function exchangeArray($data)
	{
```