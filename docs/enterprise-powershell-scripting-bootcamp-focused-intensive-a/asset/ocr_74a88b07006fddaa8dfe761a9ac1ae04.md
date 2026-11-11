+   5. Moving on, you then use another quantifier of {5} to repeat the two hexadecimal characters with a colon at the end five times.
+   6. The final part of the expression is matched from the ending anchor of $.
+   7. The ending anchor is validating to see if each character has valid hexadecimal [0-9a-f] values and uses a quantifier to specify only two characters as the ending of the string.
+   8. When the -match operator evaluates the string 00:a0:f8:12:34:56, it will return True.

You continue to evaluate the e-mail address of brent@testingdomain.com with the -match operator:

+   1. You start by using the anchor of ^ to start evaluating from the beginning.
+   2. You specify the . character to denote that anything can be contained as the beginning of the email address.
+   3. You use the +@ to evaluate the literal character of @.
+   4. You then specify the range of [^\.] , which specifies that the first character after the @ must not be a period.
+   5. It then continues to evaluate .*, which designates that any characters can be specified.
+   6. The expression then specifies \. to allow for a single period after the top-level domain is specified.
+   7. Finally, you specify the character set range of [a-z] with a minimum of two characters, specified by {2,}.
+   8. The final part of the expression is matched from the ending anchor of $.
+   9. When the -match operator evaluates the brent@testingdomain.com string, it will return True.

You then evaluate the visa credit card of 4000-4000-4000-4000 with the -match operator:

+   1. You start by using the anchor of ^ to start evaluating from the beginning.
+   2. You specify \d to designate all digits.
+   3. You then provide the range of {4} to allow for exactly four digits.
+   4. You use the - character as the number separator followed by \d{4} to provide an additional four digits.
+   5. You repeat the - character as the number separator followed by \d{4} an additional two times.