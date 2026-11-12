```
```php
$validatorChain = new ValidatorChain();
$validatorChain->attach(new StringLength
    (['min' => 3]));
$input->setValidatorChain($validatorChain);
$inputFilter->add($input);
```
```

Observe that we have created an instance of `ValidatorChain`. `ValidatorChain` connects a group of validators and allows you to apply all these validators to a value. In sequence, these validators are attached to `ValidatorChain`.

You can find the reference about Laminas standard filter classes at https://docs.laminas.dev/laminas-validator/standard-validators. For the `Discount` class, we have used the `StringLength` validator. This validator ensures that a text has a minimum and/or maximum length.

We will add validation in the inserting and updating methods of `DiscountAPIController`. `DiscountAPIController->create` must look like this:

```
```php
public function create($data)
{
    $discount = new Discount();
    $inputFilter = $discount->getInputFilter();
    $inputFilter->setData($data);
    if (!$inputFilter->isValid()) {
        return new JsonModel(['inserted' => 'invalid']
            );
    }

    $discount->exchangeArray($data);
    $inserted = $this->discountTable->save($discount);
    return new JsonModel(['inserted' => $inserted]);
}
```
```

Observe that we applied the validation rule for the discount name before trying to save the data. The database could have business rules too, but validation in the application avoids database errors and unnecessary communication between the application and the database. We get the `InputFilter` instance with the `Discount->getInputFilter` method, which makes sense. The `InputFilter->setData` method assigns the values to be validated and the `InputFilter->isValid` method answers if all values are valid according to the validator chain.