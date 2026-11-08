# 7

# Something is Wrong - Testing and Dealing with Errors

In the last chapter we learned about a powerful tool to increase the reuse and readability of our code. This tool is a combination of observables to create new sources of data. We also learned how we can run multiple asynchronous observables in parallel.

The most important operators we learned in the previous chapter are:

+   - `concat()`
+   - `merge()`
+   - `forkJoin()`
+   - `zip()`

The use of this operators can help us to avoid code repetition, giving us one very important mantra for readable code: **Don't Repeat Yourself (DRY)**.

In this chapter, we will focus on two very important issues, not only for programs implemented using functional reactive programming, but for all programs:

+   - Dealing with errors
+   - Testing our code
+     - We will see what happens to an observable when an error occurs and what we can do to treat this error and ensure our code keeps running as expected.