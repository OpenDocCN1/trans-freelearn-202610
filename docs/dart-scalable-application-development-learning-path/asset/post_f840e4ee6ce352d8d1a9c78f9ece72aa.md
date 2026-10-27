
## How to do it...

See noSuchMethod in action in the nosuchmethod project:

```
void main() {
  var embr = new Embrace(5);
  print(embr.missing("42", "Julia")); // is a missing method!
}

@proxy
class Embrace {
  // see code in previous recipes
  @override
  noSuchMethod(Invocation msg) => "got ${msg.memberName} "
      "with arguments ${msg.positionalArguments}";
}
```

This script gives the output **got Symbol("missing") with arguments [42, Julia]**.

## How it works...

When missing is called and not found, noSuchMethod is found and executed instead. The exact signature of the method is noSuchMethod(Invocation msg). When it is invoked, an object msg of type Invocation is passed to it, which contains the names of the method and its arguments. If noSuchMethod returns a value, that value becomes the result of the original Invocation. Invocation also has Boolean getters isMethod, isAccessor, isGetter, and isSetter to find out whether the called method was a normal method, getter, or setter. With this information being passed, we could do something more useful than just print it, as shown in the following code:

```
noSuchMethod(Invocation msg) =>
  msg.memberName == #meth1 ? Function.apply(meth2,
                              msg.positionalArguments,
                              msg.namedArguments)
                          : super.noSuchMethod(msg);
```

Here, we check whether the called method was meth1 (notice that memberName is of type Symbol) and if so, we call the meth2 method by passing the supplied arguments. Calling meth2 is done through the static method apply() in the Function class, which allows functions to be called in a generic fashion. Use the @override annotation to indicate that you are intentionally overriding a member. Use the @proxy annotation on the class header itself to avoid analyzer warnings if you use noSuchMethod() to implement every possible getter, setter, and method for a class.

## There's more...

Another good reason to use noSuchMethod is to reduce boilerplate code when writing a lot of similar methods. To make sure that code completion still works, use the following structure:

```
abstract class Class1Api {
  method1();
  method2();
  //...
}
```

```
class Class1 implements Class1Api {
  noSuchMethod(Invocation inv) {
    //...
  }
}
```

```
main() {
  Class1 cl = new Class1();
  // cl.  // remove comment and type cl. to see method1()
          // and so on in code completion
}
```

All the methods that will implement in noSuchMethod are summed up in the abstract class Class1Api, and then the code completion list will present them. See the code in the boilerplate.dart file and the following screenshot:

![](img/52c05b53a56abd53584e4fed738ed509_646_0.png)

## How to do it...

- The following is the code to give our class JSON functionality:
```
import 'dart:convert';

class Job {
    String type;
    int salary;
    String company;
    Job(this.type, this.salary, this.company);
```
- The following is the code to encode or serialize data:
```
String toJson() {
    var jsm = new Map<String, Object>();
    jsm["type"] = type;
    jsm["salary"] = salary;
    jsm["company"] = company;
    var jss = JSON.encode(jsm);
    return jss;
}
```
- The following is the code to decode or deserialize data:
```
Job.fromJson(String jsonStr) {
    Map jsm = JSON.decode(jsonStr);
    this.type = jsm["type"];
    this.salary = jsm["salary"];
    this.company = jsm["company"];
}
```

```
void main() {
    var job = new Job("Software Developer", 7500, "Julia Computing LLC");
    var jsonStr = job.toJson();
    print(jsonStr);
    var job2 = new Job.fromJson(jsonStr);
    assert(job2 is Job);
    assert(job2.toJson() == jsonStr);
}
```

The output of jsonStr is {"type":"Software Developer","salary":7500,"company":"Julia Computing LLC"}.

The assert statements confirm that the decoded object is of type Job and is equal to the JSON string we started from.

## How it works...

A JSON string can be stored in a file or database, or sent over the network to a server. So our class needs to be able to:

- Write its objects out in JSON format, which is also called serializing or encoding; we'll conveniently call this method `toJson()`
- Read a JSON string and construct an object (or many objects) from it; this is called deserializing or decoding, and we'll make a method `fromJson()` to do just this

Part of the work is done by functions in the imported `dart:convert` library, which produces and consumes JSON data, respectively:

- `JSON.encode()`: This serializes a Dart object into a JSON string, ready to be stored or sent over a network
- `JSON.decode()`: This builds Dart objects from a string containing JSON data, which is just read from storage or received over the network

These functions on JSON (which is an object of the class `JSONCodec`) can process data from the types `null`, `num`, `bool`, `String`, `List`, and `Map` automatically and also from a combination of these (the keys of the map need to be strings). In our `Job` class, the data is processed, as explained in the following points:

- `toJson()` makes the map from the object and then calls `JSON.encode()` on it to return a JSON string
- `fromJson()` (conveniently implemented as a named constructor) takes the JSON string, calls `JSON.decode()` to return a map, and builds the object

## There's more...

If your data is not that complicated, you can build the JSON string in the code yourself, possibly as a getter, as shown in the following snippet:

```
String get toJson => '{"type": "$type", "salary": "$salary", "company": "$company" }';
```

> If you have to construct JSON strings literally in your code, make sure to always use double quotes to indicate strings; Dart single quotes cannot be used here.

When an object contains other objects (composition or association, such as a `BankAccount` object containing a `Person` object for the owner), the `fromJson()` and `toJson()` methods from the outer object will call the corresponding methods with the same name for all the contained objects.

If you want a more sophisticated solution that supports the encoding and decoding of arbitrary objects, look for the jsonx package on pub, by Man Hoang. This library can decode a JSON string into a strongly typed object, which gets type checking and code completion support, or encodes an arbitrary object into a JSON string. When working with JSON, it is preferred to validate it; this can be done online at http://jsonlint.com/.

## Creating common classes for client and server apps

In distributed apps such as the client-server pattern, you often want to work with the same model classes on both ends. Why? Because client input needs to be validated at the client side and for this, we need the model in the client app. Data has to pass through the model before being stored so that if we want to store the data in a client database (indexed_db) as well as in a server data store, we need the model on both the sides.

## How to do it...

You can see how common classes are created for client and server apps in the client_server_db app. This is a to-do application; the client is started from web/app.html and the server is started from bin/server.dart. The client stores the to-do data in indexed_db, while the server stores the data in memory. Both client and server need the model classes.

The project structure is shown in the following screenshot:

![](img/52c05b53a56abd53584e4fed738ed509_650_0.png)

Common library

## How it works...

If you want to be able to import your common model library from another package, the files must be under the `lib` directory. In this example, we see from `pubspec.yaml` that the name of the app is `client_server`. The model (with the Task and Tasks classes) has its own folder `lib/model`, which contains the model classes defined in the `shared_model` library within `lib/shared_model.dart`. The client-app web/app.dart uses `indexed_db`; it therefore imports a library `idb_client` to work with indexed_db, as shown in the following code:

```
import 'package:client_server/idb_client.dart';
```

The `idb_client` library, defined in `lib/idb_client.dart`, imports the `shared_model` library, as shown in the following code:

```
library idb_client;

import 'package:client_server/shared_model.dart';
import 'dart:async';
import 'dart:html';
import 'dart:indexed_db';
import 'dart:convert';

part 'idb/idb.dart';
part 'view/view.dart';
```

In this way, the client app knows about the model. The server app also knows about the model by importing it (see `bin/server.dart`):

```
import 'dart:io';
import 'dart:convert';
import 'package:client_server/shared_model.dart';
// rest of code
```
