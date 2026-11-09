# Example three – casting or converting MySQL string data to a decimal number

In this example, let's consider how to convert numbers hiding inside a text column into a format suitable for calculations.

Suppose we were interested in extracting the price for a barrel of oil (abbreviated bbl) from some e-mail messages sent to Enron from a mailing list. We could write a query such that every time we see the /bbl string in an e-mail message from a certain sender, we look for the preceding dollar sign and extract the attached numeric value as a decimal number.

Here is a sample snippet from an e-mail message in the Enron message table with a message ID (mid) of 270516, showing how the number looks inside the string:

```
March had slipped by 51 cts at the same time to trade at $18.47/bbl.
```

The MySQL command to perform this string extraction and conversion to a decimal is as follows:

```
SELECT convert(
    substring_index(
        substring(
            body,
            locate('$',body)+1
        ),
        '/bbl',
        1
    ),
    decimal(4,2)
) as price
FROM message
WHERE body LIKE "%$%" AND body LIKE "%/bbl%" AND sender =
    'energybulletin@platts.com';
```

The WHERE clause restrictions are added so that we make sure we are only getting messages that have bbl oil prices in them.

The convert() function is similar to cast() in MySQL. Most modern database systems will have a way of converting data types into numbers like this.