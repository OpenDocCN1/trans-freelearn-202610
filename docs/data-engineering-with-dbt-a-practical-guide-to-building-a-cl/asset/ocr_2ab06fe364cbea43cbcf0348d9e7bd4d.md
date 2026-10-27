# Technical requirements

This chapter builds on and continues the example we started in Chapter 5, so you need to be familiar with it to follow and have the code from it to continue with the hands-on examples.

All code samples of this chapter are available on GitHub at https://github.com/PacktPublishing/Data-engineering-with-dbt/tree/main/Chapter_06.

# Writing code for humans

In the previous chapter, we loaded our first data and built our first model to transform it.

With so little code developed in our project, there is no risk of making any confusion or calling the same things with different names, yet these are real concerns in any real project, and they matter so much more than most people think.

Most people believe that when coding, you just write new code, but the reality is that to write one row of new code, you often must go back and check what you have done in a few other places, which means reading tens of rows again and again in existing code.

This becomes truer and more evident the older a project becomes, and the more people become involved in the project.

We have seen in previous chapters that there are some patterns to minimize and keep in check this issue, but remember that size is one dimension of complexity, and we can only do so much to manage it.

Removing duplicated or copied and pasted code is the top priority, as it limits the size of the project and makes evolution possible and maintenance much easier.

The other most important things that we can do to keep complexity in check are as follows:

+   - *Reduce the need to re-read*, applying the **Least Surprise Principle (LSP)**.
+     To do so, it is beneficial to use meaningful model names, prefixes, and suffixes; use a good, layered architecture and apply patterns in a consistent way (and as few as you can) so that models behave and fields mean as you would expect from their name.
+   - *Make the code that you write easy to re-read/understand.*
+     To achieve this, it is beneficial to use good **Common Table Expression (CTE)** names and column names and put everything not standard at the top of the file, so that every “deviation from expected” is as easy to spot as possible.