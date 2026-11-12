In the next section, you will learn how to create an axis with axis generators. We will have to scale data points in our dataset along the axis; thus, we will also look into scales. You will learn about linear scales, ordinal scales for non-numeric data, and time scales for time series data.

In the last section, we will apply all the gathered knowledge and create a simple bar chart containing bars (of course), axis, scales, and labels.

# Getting started with D3.js

In this chapter, we will create our first bar chart using D3.js and SVG; however, to get there, we will have to go through some basics. These basic concepts are very important because they will give you the freedom to create visualizations for almost everything that you have in your mind. The more of them you know and understand, the more crazy things you can do with D3.

D3.js is a powerful JavaScript library for data-driven DOM manipulations and includes predefined abstractions, transformations, and helpers to work with SVG elements. This makes it an excellent library to write interactive data visualization components using web standards and the complete capabilities of modern web browsers.

Don't worry if you think that this definition is full of fancy buzzwords because in this section, we will walk through all of them. You will understand exactly what they mean and why this makes D3 and SVG a great combination to create visualizations.

# Selecting and manipulating DOM elements

D3 lets us select, append, insert, and remove elements in the DOM tree. These functions will look very familiar if you've already worked with other JavaScript frameworks, such as jQuery.

Let's look at some examples to see how this works in D3. We define a simple shopping list in HTML that contains a few items that we need to buy:

```
<ul>
<li>Ham</li>
<li>Cheese</li>
<li>Butter</li>
<li>Bread</li>
</ul>
```