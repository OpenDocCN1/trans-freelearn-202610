# Testing with random inputs and pattern-based testing

It often seems like reinventing the wheel while writing test cases. Always trying to use some handwritten random inputs and trying a few of those inputs on the newly written functions. In this recipe, you will be presented with the Clojure equivalent of QuickCheck patterns, which is a famous testing framework for the Haskell programming language. No need to learn Haskell today, but let's review some cool testing patterns!

# Getting ready

In this recipe, one of the main goals is to generate various kinds of data. To achieve that, we will have a mix of handmade data generators and some others will come through libraries. Let's go through the libraries setup.

# Libraries

The `project.clj` file is a bit dense today. We will use the `midje` library not for testing but for its `autotest` feature. The `autotest` feature lets you rerun the tests automatically while writing code, and get test reports.

Compojure is not to present anymore. We will use it a bit to generate randomly generated routes, and compare routing results.

The three main libraries - `test.check`, `test.chuck`, and `herbert` - are composing the main course of this recipe.

`test.check` is the base of the Clojure version of QuickCheck, the testing framework. `test.chuck` brings some more utilities and methods to `test.check`, and finally `herbert` brings schema-based random value generation.

That gives the following: