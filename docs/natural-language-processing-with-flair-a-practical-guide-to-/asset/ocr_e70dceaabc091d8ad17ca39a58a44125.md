We now have a high-level understanding of how hyperparameter tuning works. Of course, in the real world, hyperparameters aren’t used for finding the minima of simple quadratic functions. Let’s find out how they’re used for training sequence tagging models in Flair.

# Hyperparameter optimization in Flair

As we learned in *Chapter 5, Training Sequence Labeling Models*, the success of model training often depends on a potentially large number of correctly set hyperparameters. This creates the need for finding a set of hyperparameter values that yield optimal performance. We can do this using hyperparameter optimization. Luckily, Flair offers hyperparameter tuning out of the box. Let’s learn how to do it.

Hyperparameter optimization in Flair is essentially a wrapper around Hyperopt, which we briefly covered in the previous section. The extra advantage of this wrapper is that it already feeds some sequence tagging specific information into Hyperopt so that we don’t have to. In the bare-bones Hyperopt coding exercise, we had to provide all three objects: search space, optimization method, and an objective function. But in Flair, we only need to define the search space and the framework will do the rest.

# Hyperparameter tuning in Flair hands-on

Hyperparameter optimization in Flair is best explained through code. So let’s do a simple coding exercise where we train an English PoS tagger and use Flair’s hyperparameter optimization feature to find the best model’s hidden size and learning rate.

We will achieve this by going through the following steps:

+   1. Loading the corpus
+   2. Defining the search space
+   3. Defining the `SequenceTaggerParamSelector` object
+   4. Setting off the optimization process

We first, just as with the sequence tagging exercise in *Chapter 5, Training Sequence Labeling Models*, need to load the English Corpus object into memory. It contains a tagged English corpus, split into the test, train, and dev datasets:

```
from flair.data import Corpus
from flair.datasets import UD_ENGLISH

corpus = UD_ENGLISH()
```