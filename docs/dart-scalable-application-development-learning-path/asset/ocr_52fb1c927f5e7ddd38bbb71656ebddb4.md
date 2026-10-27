```
zero: "No one occurrence replaced for $str",
    one: "$num occurrence replaced for $str",
    two: "$num occurrence replaced for $str",
    few: "$num occurrences replaced for $str",
    other: "$num occurrences replaced for $str")}"",
    name: "replace",
    args: [num, str],
    desc:"How many occurrences replaced for string",
    examples: { 'num':2, 'str':'hello'},
);
```

# Adding gender

The gender method of the Intl class provides out-of-the-box support for gender-based selection, as shown in the following code:

```
String usage(String name, String gender, String car) =>
    Intl.gender(
        gender,
        male: "$name uses his $car",
        female: "$name uses her $car",
        other: "$name uses its car",
        name:"usage",",
        args: [name, gender, car],
        desc: "A person uses the car.");
```

The gender parameter must equal to one of the literal values: male, female, or other. This method can be used as a part of the Intl.message method:

```
String usage(String name, String gender, String car) =>
    Intl.message(
        """${Intl.gender(
            gender,
            male: "$name uses his $car",
            female: "$name uses her $car",
            other: "$name uses its car")}""",
        name: "usage",
        args: [name, gender, car],
        desc: "A person uses the car.");
```