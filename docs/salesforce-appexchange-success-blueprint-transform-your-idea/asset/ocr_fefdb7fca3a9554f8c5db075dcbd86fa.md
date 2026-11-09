Let’s delve into our tax calculation scenario. We’ll see how, using Apex DI, it’s tailored to support local rules defined by our users.

# 1. Interface

First, we’ll define a global interface for tax calculation, as follows:

```
global interface ITaxCalculator {
    Decimal calculateTax(Decimal amount);
}
```

# 2. Default class (part of your managed package)

Next, we’ll create a default class that implements the interface. This class will be part of a managed package:

```
public class DefaultTaxCalculator implements ITaxCalculator {
    public Decimal calculateTax(Decimal amount) {
        return amount * 0.05; // 5% default tax
    }
}
```

# 3. Custom metadata

Within your managed package, introduce a new subscriber-editable custom metadata file named TaxCalculatorMapping__mdt. For the sake of simplicity in this example, we’ll utilize the default fields, as follows:

+   - MasterLabel to store country codes (for example, US)
+   - DeveloperName to capture the class name (for example, DefaultTaxCalculator)

In a real-world scenario, it would be more appropriate to create custom fields tailored to these specific needs.