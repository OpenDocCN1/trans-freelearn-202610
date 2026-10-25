The custom error would still work or be consumed for throwing invalid domain validation errors as well:

![](bbox=[0.104, 0.168, 0.896, 0.548])

The displayed error message will be cleared once the user starts typing a valid email value into the field.

The valid email value was decided by checking the @ symbol of the value; mysteriously, it clears the validation (test@somedomain) before the user completes the email with a valid domain name (gmail.com or yahoo.com). Once the . value is provided again, it displays the error message, expecting the user to provide any domain extension value such as .com or .net (it works perfectly, even if we provide .abc).