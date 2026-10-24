# # Optional LAMBDA Parameters

Cell **F11** of the `LAMBDA` function worksheet, as shown in _Figure 11.22_, contains
`=LAMBDA(quantity,price,[tax],(price+IF(ISOMITTED(tax),0,price*tax))*quantity)`
formula, which has an optional `[tax]` **Parameter**, as indicated by the
square brackets:

![](bbox=[0.1309, 0.2266, 0.8535, 0.4359])

The formula also uses the `ISOMITTED` function, which returns `TRUE` if the **Parameter** being tested
was not provided, or `FALSE` if the **Parameter** is specified. This calculation is like what we carried out
with the `LET` function. The `calculation` argument for the `LAMBDA` function takes the `price`
**Parameter** and determines if `tax` should be calculated or not. If no `tax` **Parameter** is provided, the
`IF` function returns `0`; otherwise, it multiplies `price` by `tax`. The result that `IF` returns is added
to `price` multiplied by `quantity`.

Cell **F12** adds a function call of `(B12,C12)` that passes `B12` to `quantity` and `C12` to `price`,
but omits `tax`, so the formula returns `1,000`.

Cell **F13** adds a function call of `(B13,C13,C14)` that passes **B13**, **C13**, and **C14** to `quantity`,
`price`, and `tax`, respectively, so the formula adds tax and returns `1,070`.

The `IF` function in these formulas illustrates how to incorporate existing Excel worksheet functions
into `LAMBDA`. Simply create a **Parameter** for every argument that an existing worksheet function
requires, plus any other inputs that you want to include.

Now, let’s see what happens if you assign the **Name** of an existing worksheet function to a `LAMBDA`
function.