
## Grid search

This is a type of exhaustive search (meaning it explores the entire search space) that simply scans through all possible combinations of the predefined parameter options. It clearly suffers from the curse of dimensionality. The upside of grid search is that it can be fully parallelized.

## Random search

A non-exhaustive search algorithm that simply samples random hyperparameter values and evaluates each combination. It repeats this process for a specified number of steps and returns the set of hyperparameter values that result in the best-performing model.

## Bayesian optimization

A technique that uses Bayes' theorem to perform a search in order to find the optimal set of hyperparameters. It builds a probabilistic model that maps the hyperparameter value to the `objective` (loss) function.

A special variation of Bayesian optimization is the **Tree-structured Parzen Estimator** (**TPE**). It is often used in the popular Python hyperparameter optimization `hyperopt` library. The library's source code and documentation can be found at `https://github.com/hyperopt/hyperopt`.

## Evolutionary algorithms

Another take on hyperparameter optimization methods is the evolutionary algorithm approach. The algorithm follows these steps:

- 1. Generate a population of randomly chosen hyperparameter values.
- 2. Train and evaluate a model for each individual member of the population.
- 3. Replace the lowest-scoring individuals with newly formed individuals that are mutated or crossed-over variations (meaning a combination of multiple individuals) of individuals that performed well.
- 4. Jump to step 2 and repeat until the desired score is obtained, no improvement is visible, or an iteration limit is reached.

We now have a basic understanding of how hyperparameter tuning works. But all these new ideas and concepts may sound a little daunting if we imagine implementing them by hand. Luckily, there are many ways to leverage the readily available libraries in Python that do just that.

## Hyperparameter tuning in Python

Let's get a taste of what hyperparameter tuning looks like in practice. We will use one of the most popular Python hyperparameter optimization libraries called Hyperopt. First, let's get a general idea of how to use it in practice.

Hyperopt is a Python library that provides an easy-to-use API that requires the following three objects:

- A search space
- An objective function
- An optimization method

Let's look at each of these requirements in detail:

- **Search space** is simply the space within which the optimizer will search for different hyperparameter options. The library provides the following parameter expressions:
  - **Categorical parameters** – Parameter values that are purely categorical and can even be non-scalar (they do not need to be a number). They are provided via the `hyperopt.hp.choice` method.
  - **Integer parameters** – Integer value parameters obtained via methods such as `hyperopt.hp.quniform` or `hyperopt.hp.qlognormal`.
  - **Float parameters** – Decimal floating-point numbers obtained via methods such as `hyperopt.hp.normal` (which samples from normal distribution) and `hyperopt.hp.uniform` (which samples from uniform distribution).
- **An objective function** that determines how well a certain hyperparameter value combination performs. In deep learning, this is most often the loss function.
- **An optimization method** is an algorithm for performing a search within the search space. The options in hyperopt are: random search (`hyperopt.rand.suggest`), TPEs (`hyperopt.tpe.suggest`), and Adaptive TPEs (`hyperopt.atpe.suggest`).

The three preceding concepts are really all Hyperopt needs to find the optimal hyperparameters for you. Let's put this into practice as part of a hands-on exercise in Python.

## Hyperparameter tuning in Python hands-on

To illustrate how hyperparameter tuning works in its simplest form, we will come up with a very simple artificial problem. We will optimize a single hyperparameter using a simple objective function that depends solely on that single hyperparameter, and we already know the most optimal (minimum) value for it. Let's pick this simple quadratic function:

$f(x) = x^2 + 1$

![](img/c56c48b8fd18a11ab705352d618ed976_102_0.png)

Figure 6.1 – The $x^2 + 1$ function

It's clear that the minimum of $x^2 + 1$ is 1 at $x = 0$. This now allows us to define the objective function in Python:

```
def objective(value):
    return value ** 2
```

We can then define the search space for a single hyperparameter. To keep things simple, we will choose uniform distribution and sample a range where we know the minima is, say, between -100 and 100:

```
from hyperopt import hp
```

```
# define the search space for a single hyperparameter
space = hp.uniform('param1', -100, 100)
```

The preceding code snippet will execute successfully if you have previously installed Flair. If not, make sure to run pip3 install flair==0.11 in the console before running.

Finally, we can now choose the optimization method:

```
from hyperopt import tpe

# the Tree of Parzen Estimators (TPE) algorithm
optimization_method = tpe.suggest
```

With all three required objects defined, we can initiate the optimization process and print out the results:

```
from hyperopt import fmin

# minimize the objective function over 1000 evaluations
best_params = fmin(objective,
                space,
                algo=optimization_method,
                max_evals=1000)

# print the best hyperparameter value
print(best_params)
```

The preceding code snippet prints out the following:

```
{'param1': 0.01084844145467706}
```

This indicates that, according to Hyperopt, the optimal value for our objective function is 0.01084844145467706. This value is very close to the actual optimal value, which we already know is 0. The obtained value is also closer to the actual optimal value (0) than the expected optimal values of a random search. This means that TPE did a good job of optimizing our objective function.

## Hyperparameter optimization in Flair

We now have a high-level understanding of how hyperparameter tuning works. Of course, in the real world, hyperparameters aren’t used for finding the minima of simple quadratic functions. Let’s find out how they’re used for training sequence tagging models in Flair.

As we learned in *Chapter 5, Training Sequence Labeling Models*, the success of model training often depends on a potentially large number of correctly set hyperparameters. This creates the need for finding a set of hyperparameter values that yield optimal performance. We can do this using hyperparameter optimization. Luckily, Flair offers hyperparameter tuning out of the box. Let’s learn how to do it.

Hyperparameter optimization in Flair is essentially a wrapper around Hyperopt, which we briefly covered in the previous section. The extra advantage of this wrapper is that it already feeds some sequence tagging specific information into Hyperopt so that we don’t have to. In the bare-bones Hyperopt coding exercise, we had to provide all three objects: search space, optimization method, and an objective function. But in Flair, we only need to define the search space and the framework will do the rest.

## Hyperparameter tuning in Flair hands-on

Hyperparameter optimization in Flair is best explained through code. So let’s do a simple coding exercise where we train an English PoS tagger and use Flair’s hyperparameter optimization feature to find the best model’s hidden size and learning rate.

We will achieve this by going through the following steps:

- 1. Loading the corpus
- 2. Defining the search space
- 3. Defining the `SequenceTaggerParamSelector` object
- 4. Setting off the optimization process

We first, just as with the sequence tagging exercise in *Chapter 5, Training Sequence Labeling Models*, need to load the English Corpus object into memory. It contains a tagged English corpus, split into the test, train, and dev datasets:

```
from flair.data import Corpus
from flair.datasets import UD_ENGLISH

corpus = UD_ENGLISH()
```
