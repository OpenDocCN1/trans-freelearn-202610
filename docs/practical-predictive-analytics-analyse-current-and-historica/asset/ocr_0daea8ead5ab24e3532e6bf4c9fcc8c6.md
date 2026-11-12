Open up a new script window, and run the following code:

```
#GENERATE MEMBER
set.seed(1010)
member <- r_data_frame (
  n=1000,
  r_sample_replace(x = 1:1000,replace=FALSE,name="memberid"),
  age,
  gender(x = c("M","F"), prob = c(.5,.5),name="Gender"),
  dob,
  income,
  children,
  employment,
  level,
  grade,
  year,
  state,
  zip_code
)
```

Similarly, generate the purchases file, and assign it to the purchases
dataframe. The total amount purchases will come from a normal distribution
with a mean purchases amount of 20,000 and a standard deviation of 1,000.
The Product variable is a random product name coming from the letters A-Z.

This next code snippet will generate the purchases:

```
#GENERATE PURCHASES
set.seed(1010)

purchases <- r_data_frame(
  n=1000,
  r_sample_replace(x = 1:1000,replace=TRUE,name="memberid2"),
  purch=rpois(lambda=1),
  normal(mean = 20000, sd = 1000, min = NULL, max = NULL, name =
  upper(x = LETTERS, k=3, prob = NULL, name = "Product")
)
purchases$purch <- purchases$purch + 1
str(purchases)
```

After generating the purchases dataframe, we will write it to an external CSV
file. The reason we are doing this is to demonstrate how we can filter rows
from an external file using read.csv.sql: