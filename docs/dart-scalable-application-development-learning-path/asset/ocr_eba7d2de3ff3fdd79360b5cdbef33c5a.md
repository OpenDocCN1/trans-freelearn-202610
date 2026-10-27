# Static generation

Web hosting facilities vary greatly from provider to provider. The ability to run programs and install software is not universally available. Static generation of websites has become more popular with the ability to run small simple virtual machines on the cloud.

The advantages are very clear, there is not maintenance on software or a database to maintain, making the system very secure and also very fast to serve. It is not suitable for all applications, though. Blogging is one where static generation may make sense.

# Freezing the website

In this chapter's project, there is a staticgen.dart file in the bin folder. This is a standalone program that generates a static HTML file of the front page of the blog. Note that the main function is marked with the async keyword and is missing the void keyword.

# Introducing the await and async keywords

The await and async keywords were introduced in Dart 1.9.4 in order to simplify asynchronous operations. Consider the following call to a method that returns Future:

```
obj.method().then(handlerFunction)
```

This is fine for the post part, but what if things get more complicated and handlerFunction returns a future too?

```
obj.method().then(handlerFunction).then(handlerFunction2);
```

Things are starting to get complicated already – debugging is not straightforward. Ideally, we would want to deal with one part of the chain at a time and hold up the execution of statements until a desired operation is complete. This is what await allows:

```
var f1 = await obj.aMethod();
var result = await f1.aMethod();
```

Functions and methods to be called with await return Future. They must also be declared as async in the method's header, as does the function using the await call:

```
class Foo{
  Future<int> aMethod() async {
    return await aFunction();
  }
}
```