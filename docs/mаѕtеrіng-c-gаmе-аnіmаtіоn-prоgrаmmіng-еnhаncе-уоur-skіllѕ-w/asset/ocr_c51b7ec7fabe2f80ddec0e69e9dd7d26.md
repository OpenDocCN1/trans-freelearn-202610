# 3

# Adding a Visual Selection

Welcome to Chapter 3! In the previous chapter, we offloaded the majority of matrix and vector calculations to the GPU. A modern graphics card has more (and also more specialized) computing cores than a desktop CPU, so, moving the compute load to the GPU will free the main CPU from most of the animation work.

In this chapter, we will add a couple of simplifications when working with a lot of model instances. After the changes in the previous chapter, we are able to display thousands of model instances on the screen, but selecting a specific instance is still hard. We will start by adding coordinate arrows to identify the currently selected instance. Next, we will add a function that allows us to center the specified instance in the middle of the screen. Then, a graphical highlight will be created, further helping us to find the selected instance among all instances. As the last step, we will add a model without triangles, and an instance from this empty model, allowing us to deselect the visible instances.

In this chapter, we will cover the following topics:

+   - Implementing a “move to instance” function
+   - Adding a highlight to the selected instance
+   - Selecting a model instance with point and click
+   - Implementing a null object to allow deselection

At first glance, these topics may look unrelated to animation programming. But proper tooling is an essential part of creating a user-friendly application. Good tools will help the user to simplify application handling.