The syntax to declare a function is shown as follows:

```
function <nameofthefunction> (<parameter types> <name of the variable>)
{internal|external} [constant] [payable] [returns (<return types> <name of the variable>)]
```

# Reference types

As the name suggests, these types are passed by reference and are discussed in the following section.

# Arrays

Arrays represent a contiguous set of elements of the same size and type laid out at a memory location. The concept is the same as any other programming language. Arrays have two members named length and push:

```
uint[] OrderIds;
```

# Structs

These constructs can be used to group a set of dissimilar data types under a logical group. These can be used to define new types, as shown in the following example:

```
Struct Trade
{
uint tradeid;
uint quantity;
uint price;
string trader;
}
```

# Data location

Data location specifies where a particular complex data type will be stored. Depending on the default or annotation specified, the location can be storage or memory. This is applicable to arrays and structs and can be specified using the storage or memory keywords. As copying between memory and storage can be quite expensive, specifying a location can be helpful to control the gas expenditure at times. Calldata is another memory location that is used to store function arguments. Parameters of external functions use calldata memory. By default, parameters of functions are stored in memory, whereas all other local variables make use of storage. State variables, on the other hand, are required to use storage.