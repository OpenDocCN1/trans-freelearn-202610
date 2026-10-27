# Applications

A filtering example: To see payments made by a company for a certain drug in the state of NY:

![](bbox=[0.03, 0.2, 0.9, 0.62])

Using the RShiny Application

Note that the system processed 11 million records in 21 milliseconds, as shown in the header message. The name of the company and the product has been blanked out in the screenshot for privacy, but you are free to try out different options for both fields.

![](bbox=[0.07, 0.73, 0.14, 0.79])

Note that in the default VM, we are using only one core with very limited memory, and the speed with which the data is processed using kdb+ even on a laptop with very limited resources easily exceeds the performance of many well-to-do commercial solutions.