6. You should see a line of output in the console area containing the number 42.

![](bbox=[0.17, 0.18, 0.83, 0.35])

# What Just Happened?

We just created what is called in JavaScript an anonymous object, or object literal. Now, we have discussed how objects are created from classes, and in JavaScript we need to use the new keyword to instantiate an object from a class. But there is no new keyword here!

# Object literals

The key concept here is that we are just creating a single object that does not derive from a class. Since object literals (anonymous objects) do not derive from a class, it is, essentially, an empty object. It contains only what we explicitly define. The value associated with a key can be almost anything—a string, integer, function, array, or even another object literal.

We encountered object literals in Chapter 1 when we discussed JavaScript Object Notation—they were the {key:value} pairs used to define the parameters and options of our layer and objects. The only difference is that we did not assign a variable to them; we simply passed them in when we created our layer object.

Object literals are extremely useful for a variety of tasks, and it is a great way to package information in an easy to use form. They are in the form of {key:value, key2:value2}. We can access any property of an object literal by using dot notation, in the form of my_object_literal.key. The key, like before, is the key part of the key:value pair. In the above code, we call console.log(my_parameters.answer); and the value of the key answer is displayed in the console's log area.