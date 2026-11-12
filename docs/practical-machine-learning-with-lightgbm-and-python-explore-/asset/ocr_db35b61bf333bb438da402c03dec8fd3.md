# What is machine learning?

Machine learning is a part of the broader artificial intelligence field that involves methods and techniques that allow computers to “learn” specific tasks without explicit programming.

Machine learning is just another way to write programs, albeit automatically, from data. Abstractly, a program is a set of instructions that transforms inputs into specific outputs. A programmer’s job is to understand all the relevant inputs to a computer program and develop a set of instructions to produce the correct outputs.

However, what if the inputs are beyond the programmer’s understanding?

For example, let’s consider creating a program to forecast the total sales of a large retail store. The inputs to the program would be various factors that could affect sales. We could imagine factors such as historical sales figures, upcoming public holidays, stock availability, any special deals the store might be running, and even factors such as the weather forecast or proximity to other stores.

In our store example, the traditional approach would be to break down the inputs into manageable, understandable (by a programmer) pieces, perhaps consult an expert in store sales forecasting, and then devise handcrafted rules and instructions to attempt to forecast future sales.

While this approach is certainly possible, it is also brittle (in the sense that the program might have to undergo extensive changes regarding the input factors) and wholly based on the programmer’s (or domain expert’s) understanding of the problem. With potentially thousands of factors and billions of examples, this problem becomes untenable.

Machine learning offers us an alternative to this approach. Instead of creating rules and instructions, we repeatedly show the computer examples of the tasks we need to accomplish and then get it to figure out how to solve them automatically.

However, where we previously had a set of instructions, we now have a trained model instead of a programmed one.

The key realization here, especially if you are coming from a software background, is that our machine learning program still functions like a regular program: it accepts input, has a way to process it, and produces output. Like all other software programs, machine learning software must be tested for correctness, integrated into other systems, deployed, monitored, and optimized. Collectively, this forms the field of machine learning engineering. We’ll cover all these aspects and more in later chapters.

# Machine learning paradigms

Broadly speaking, machine learning has three main paradigms: supervised, unsupervised, and reinforcement learning.