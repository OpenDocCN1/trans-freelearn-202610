You can find the source code of this chapter at https://github.com/PacktPublishing/Full-Stack-App-Development-with-Angular-and-GraphQL/tree/main/Chapter06.

TypeScript expertise is not required, however, knowledge of concepts such as classes and decorators is helpful.

An example of a TypeScript class is as follows:

```
```typescript
class Product {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  getName() {
    return this.name;
  }
}

let product1 = new Product("product1");
```
```

If you've worked with a class-based oriented programming language before, you should be familiar with this syntax, but even recent versions of JavaScript, such as ECMAScript 6 or JavaScript 2015, enable developers to define classes using this syntax.

We simply define classes by using the class keyword, followed by the name and the body of the class.

The members of the class are added to the body, which is surrounded by braces. In this case, we have a property named name of the string type, a constructor, and a method named getName().

Finally, we create an instance of the Product class using the new keyword, which creates a new object with the Product type and invokes the constructor of the class to initialize it.

Classes may be thought of as a mechanism to create user-defined types that allow you to easily represent real-world objects.

In TypeScript, a decorator is a special construct that allows you to alter classes, methods, accessors, properties, and arguments dynamically.