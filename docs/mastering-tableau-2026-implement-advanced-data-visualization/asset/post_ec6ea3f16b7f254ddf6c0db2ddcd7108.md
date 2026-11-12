

![](img/66cf6b89349fda3581a4f196cb3c98a4_650_0.png)

Figure 14.28: Analytics Extension connection

- 7. As you can see in the previous screenshot, it is possible to set up usernames, passwords, and SSL if you have any security concerns or for a bigger enterprise-wide rollout

- 8. Click on Test Connection and you should see the following pop-up:

![](img/66cf6b89349fda3581a4f196cb3c98a4_650_1.png)

Figure 14.29: Testing the analytics extension

The full documentation on how to get TabPy up and running can be found at https://github.com/tableau/TabPy. If you don't want to or cannot install TabPy on your machine, Tableau also offers a Docker container that will install the latest version of TabPy. You can find more information here: https://hub.docker.com/r/emhemh/tabpy/.

## Implementing Python functionality

Just like R, TabPy makes use of the SCRIPT_ functions in Tableau. In the next sections, we will practice working with TabPy and will look at multiple use cases. Tableau calculations using TabPy look very similar to R’s. For TabPy, it is important to add a return statement to the calculated field and notice that arguments are noted with an underscore instead of a dot:

![](img/66cf6b89349fda3581a4f196cb3c98a4_651_0.png)

Figure 14.30: Python TabPy syntax

This will be manifested in the next exercises; we will first investigate random number generators.

### Random and random normal

Many calculations are easily accessible via the calculated fields, others via the table calculations—and then there are some hidden functions. Hidden because those are not (yet) fully supported or tested; therefore, use them with care. If you tried to find the function Random, for example, you would not be able to. But you can still use the Random() function, as can be seen here:

![](img/66cf6b89349fda3581a4f196cb3c98a4_651_1.png)

Figure 14.31: Random

Another option is using TabPy to get the Random function. We will look at random as well as random normal with the variables mu and sigma. This will let us draw random numbers from a distribution, a method often used in statistics and quantitative modeling, among other areas, to simulate, reproduce, or calculate probabilities; mu and sigma are the mean and standard deviation defining the distribution.

### Generating random numbers

Perform the following steps to create a dot cloud of random variables with a specific underlying distribution, defined by the mean and standard deviation:

- 1. Create an Excel sheet with one Index column and rows with the numbers 1-1,000. This is needed to have an initial dataset with the number of rows we want to use for the random number generator.
- 2. Save the file and connect Tableau to this Excel sheet. You should see one imported measure called **Index** (just like the header in your Excel file).
- 3. Connect Tableau to Python as described in *Python installation and integration*

4. Create a calculated field called Random, which should look as follows:

```
SCRIPT_REAL("
from numpy import random as rd
return rd.random(_arg1[0]).tolist()",
SIZE()
)
```

- 5. Drag **Index** to **Rows** and **Random** to **Columns**
- 6. Disable **Aggregate Measures** on the **Analysis** tab

![](img/66cf6b89349fda3581a4f196cb3c98a4_652_0.png)

Figure 14.32: Random visualized

In the preceding screenshot, you can see the 1,000 random data points. Let's consider some of the code used in this exercise that allowed us to generate this visualization:

| SCRIPT_REAL | This Tableau function calls the Python engine and returns a float value. |
|---|---|
| "from numpy import random as rd | In this part of the code, we need the Python library numpy, and from that library, we need the module random, which we will load with the shorter name rd. |
| return | This command is simply needed to return a value. |
| rd.random(_arg1[0]).tolist()" | In this step, we call the module rd that we imported before. From the rd module, we retrieve the random function. The _arg1[0] is needed to activate the function given a certain value. And lastly, we put all the retrieved values in a list of values by adding .tolist(). |
| SIZE() | This is the value that will be replacing _arg1, and _arg1 is required by the random function. We use SIZE() to fulfill the requirement because it will return the number of rows in the partition and is sufficient to get a random number back from the function. |

Table 14.3: A quick code recap

We got exactly 1,000 numbers because our initial Excel sheet had 1,000 rows. By using calculated fields, you can add columns to a data frame, but not rows. Therefore, we need a data source that will provide us with the data schema. In the next section, we will learn how to specify a random number drawn from a normal distribution.

### Random normal

Now, let's reproduce a random variable with a normal distribution. This technique is often used in statistical modeling to calculate probabilities. The random normal values can be used instead of or in addition to observations you already collected. Once we know how to use a normal distribution, you can extend this knowledge and create other distributions in Tableau as well:

- 1. Reuse the workbook from the previous exercise
- 2. Create a Random Normal calculated field:

```
SCRIPT_REAL("
from numpy import random as rd
mu, sigma = 0, 1
return (rd.normal(mu, sigma, _arg1[0])).tolist()
",
SIZE()

)
```

- 3. Place Random Normal on Columns and Index on Rows

- 4. Disable Aggregate Measures on the Analysis tab

- 5. Select a Marks type of Circle. You can now see a plot with the 1,000 data points normally distributed:

![](img/66cf6b89349fda3581a4f196cb3c98a4_654_0.png)

Figure 14.33: Normal distribution visualized

- 6. To give your users more flexibility, you can also add parameters to your view that interact with the Python integration. For example, create the following two parameters:

![](img/66cf6b89349fda3581a4f196cb3c98a4_654_1.png)

Figure 14.34: The sigma and mu parameters

- 7. Then, create a calculated field called Random Normal Param like:

```
SCRIPT_REAL("
from numpy import random as rd
mu, sigma = _arg2, _arg3
return (rd.normal(mu, sigma, _arg1[0])).tolist()
",
SIZE(), [mu], [sigma]
)
```

- 8. Add the parameter control to your view so your users can decide which variables they want to pass to Python:

![](img/66cf6b89349fda3581a4f196cb3c98a4_655_0.png)

Figure 14.35: Final normal distribution worksheet

Compared to our prior visualization with the normal distribution, you can see in the preceding screenshot that by changing the mu parameter to 10, we change the mean to 10 and thus can move the whole dot cloud in any direction. With the random number being available in Tableau, you can, for example, visualize a Monte Carlo simulation, which is used in many real-life applications because it can model possible probability outcomes and thus helps interested parties understand the impact of risk and uncertainty.

> **Note**
>
> More information on how to calculate a Monte Carlo simulation in Tableau can be found here -- https://jacksontwo.com/exploring-python-tableau – and more general Monte Carlo content can be found here: https://www.ibm.com/topics/monte-carlo-simulation.

Before moving on, let's again consider some of the key lines of code that were used in this exercise:

| SCRIPT_REAL | This Tableau function calls the Python engine and returns a float value. |
|---|---|
| "from numpy import random as rd | In this part of the code, we need the Python library numpy, and from that library, we need the module random, which we will load with the shorter name rd. |
| mu, sigma = _arg2, _arg3 | This part defines that we will refer to mu and sigma with _arg2 and _arg3. |
| return | This command is simply needed to return a value. |
| rd.normal(mu, sigma, _arg1[0]).tolist() | In this step, we call the module rd that we imported before. From the rd module, we retrieve the random function. The _arg1[0] is needed to activate the function given a certain value. Optional values are mu and sigma. This time, we will use those two as well. And lastly, we put all the retrieved values in a list of values by adding .tolist(). |
| SIZE(), [mu], [sigma] | This is the value that will be replacing _arg1, _arg2, and _arg3. Just like before, we use SIZE() to activate the function. The optional values mu and sigma will point to the parameter we created before. |

Table 14.4: A quick code recap

After finishing the first two exercises with TabPy, we learned how we can use a random number in Tableau. Next, we changed the random number to a random number drawn from a normal distribution and added parameters to the dashboard such that the user can change mu and sigma of the normal distribution. Of course, you can change the function in TabPy to other distributions as well.

The next topic is more advanced. We will use TabPy to execute a sentiment analysis on newspaper headlines from 2003 to 2021.

