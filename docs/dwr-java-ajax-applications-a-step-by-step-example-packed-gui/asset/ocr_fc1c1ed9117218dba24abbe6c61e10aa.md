```
for (int i = digitsOnly.length() - 1; i >= 0; i--) {
    digit = Integer.parseInt(digitsOnly.substring(i, i + 1));
    if (timesTwo) {
        addend = digit * 2;
        if (addend > 9) {
            addend -= 9;
        }
    } else {
        addend = digit;
    }
    sum += addend;
    timesTwo = !timesTwo;
}
int modulus = sum % 10;
return modulus == 0;
```

The first method, submitOrder(), takes the name, address, and credit card info as parameters and validates the input before sending the order to the order-processing system (not implemented in this sample) and returns either true or false value based on whether the order was successful or not.

The second method, checkExpiryFormat(), takes the credit card expiry date as the parameter and validates that it is in the correct format, MM/YY where MM and YY are integers in the range of 1-12 and 8-99 (years 2008-2099) respectively.

The last two methods validate that the credit card number is in the correct format. The code is written by Michael Gilleland, and it was published in his essay Anatomy of Credit Card Numbers (http://www.merriampark.com/anatomycc.htm). The essay explains what all those numbers in a credit card are, and how to verify that a given number is really a credit card number using the Luhn algorithm (refer to the essay for more information).

# Testing the Form

The next thing to do is to add FormHandler to the HTML page and the remoted class to dwr.xml. We add the following to the index.jsp file:

```
<script type='text/javascript' src='/DWREasyAjax/dwr/interface/FormHandler.js'></script>
```