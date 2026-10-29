
|  |  |
|---|---|
| Downloading Kafka 0.8.2.2 | 255 |
| Start ZooKeeper and Kafka server | 255 |
| Create topic | 256 |
| Run a console consumer | 257 |
| Using clj-kafka | 257 |
| How to do it... | 257 |
| Declaring the namespace and load Clojure library | 258 |
| Producing messages | 258 |
| Consuming a messages | 258 |
| How it works... | 259 |
| Producing messages | 259 |
| Consuming messages | 260 |
| There's more... | 262 |
| Creating a new topic | 262 |
| See also | 262 |
| Using MQTT | 264 |
| Getting ready | 264 |
| Installing the MQTT broker | 265 |
| MQTT graphical client | 265 |
| Send our first MQTT message | 269 |
| Getting some Clojure | 272 |
| How to do it... | 272 |
| How it works... | 274 |
| There's more... | 276 |
| Streaming access to provide high performance | 281 |
| Getting ready | 281 |
| How to do it... | 281 |
| Streaming out | 281 |
| Streaming in | 283 |
| Streaming with line-seq | 283 |
| How it works... | 283 |

There's more... 285
Using Apache Camel to connect everything 287
Getting ready 287
How to do it... 287
Importing Camel classes 287
Your first ride on Camel with Clojure 287
Writing more Clojure idiomatic code with Camel 288
How it works... 290
There's more... 290
See also 291
## 5. Working with Other Languages 292
### Introduction 292
Calling Java methods and accessing Java objects from Clojure 293
Getting ready 293
How to do it... 293
Instantiating objects 293
Calling instance methods 293
Calling class methods 294
Accessing instance fields 294
Accessing class fields 295
Accessing inner classes 295
Referencing classes 296
Using arrays 296
How it works... 297
Summary of how Clojure accesses Java methods and objects 298
Clojure does not inherit Java's checked exception 299
Summary of array accesses 300
Print strings of Java's primitives 300
There's more... 301
Chained calls with the double-dot macro 301
Using the doto macro 301

Using reflections 302

Extending Java superclasses and implementing Java interfaces 303

Getting ready 303

How to do it... 303

Using proxy 303

Using reify for implementing interfaces 304

How it works... 304

There's more... 305

Defining anonymous classes in Clojure 305

Using annotations 306

Calling Clojure from Java 307

Getting ready 307

How to do it... 307

Defining a simple named class using gen-class 307

Defining a class with instance methods and constructors in Clojure 308

Creating a JAR file callable from Java 310

How it works... 312

The project.clj for generating Java classes 312

AOT compilation 313

There's more... 313

Defining an interface and implementing a class callable from Java 314

Using Maven for Clojure and Java projects 315

Calling Scala from Clojure 319

Getting ready 319

How to do it... 319

Using Leiningen to develop Scala and Clojure 319

Importing Scala classes 320

Calling instance methods 320

Calling singleton methods 321

Accessing tuples 321

Accessing Scala fields 321

How it works... 322
There's more... 323
Using SBT 323
Creating an SBT-based project 323
Writing source code 324
ClojureCLR 326
Getting ready 326
Installing Visual Code Studio and creating a project 327
Installing Mono Studio IDE 332
Lein CLR 337
How to do it... 339
Calling .NET code from Clojure 339
Starting an REPL 339
Working with IO 340
Parsing some XML using C# code 341
Calling a REST API using Clojure/C# 342
UI Prototyping 344
Putting it all together 346
How it works... 347
Compiling Clojure code for .NET 347
Adding Clojure code to a Visual Studio project 349
Running Clojure code using Clojure.RT 351
Using Xamarin Mono Studio 351
There's more... 354
ClojureScript 357
Getting ready 357
How to do it... 359
Autocompiling your code 359
Fibonacci'ed 360
How it works... 360
Interacting with JavaScript 360
Using jQuery from ClojureScript 361
Creating a ClojureScript library 364
Using a third-party library 365
There's more... 366
Running in the REPL 366
Compiling code for both Clojure and ClojureScript 367
## 6. Concurrency and Parallelism 369
Introduction 369
Solving concurrent problems using Clojure 370
Getting ready 370
How to do it... 370
Using atoms 370
Creating and referring atom 370
Updating atom 371
Using validator 372
Using CAS operation 372
Software Transactional Memory using ref and dosync 374
Creating ref 374
Updating refs using alter 374
Using ensure 376
Using watcher and a refined transaction code 377
Using commute 379
Using agents 381
Creating agents 381
Updating agents 381
How it works... 382
Summary of three reference types in Clojure 382
Coordinated/uncoordinated 382
Synchronous/asynchronous 382
How STM works in Clojure 383
Alter and commute 383
There's more... 383
promise and deliver 383
pmap, pcalls, and pvalues 386
pcalls 386
pvalues 387
See also 387
Distributed actor-based dev with Akka 388
Getting ready 389
How to do it... 389
Your first okku 389
Creating actors 390
Dispatching messages 391
How it works... 392
Actor system 392
Understanding actors 393
There's more... 394
Calling remote actors 394
Request and reply 397
See also 399
Using Spyglass and Couchbase to share state between JVMs 400
Getting ready 400
Setting up dependencies in project.clj 400
Setting up Couchbase Server as a Memcached server 400
Using docker-compose to start Couchbase Server 400
Creating a directory and a file for docker-compose 401
Start Couchbase cluster 401
Check if Docker processes are running 402
Summary of Couchbase cluster 403
Using Couchbase web administrator console 403
Logging into the web console 403
Adding nodes to the cluster 405
Adding Cluster 405
Defining Memcached bucket 408
How to do it... 410
Defining to use Spyglass library 410
Connecting server 410
Set values 411
Get values from another REPL 411
Testing time to live 412
Disconnecting 412
How it works... 412
What is Memcached? 412
Access Memcached from Telnet 413
Couchbase Server 414
There's more... 415
Using get-multi 415
Using async-get 416
Using CAS operations 416
See also 420
Reactive programming with meltdown 421
Getting ready 421
How to do it... 422
Defining a reactor 422
Sending a message to a reactor 423
Going the sync way 423
How it works... 424
Selectors 424
Streams 425
Streams and reduce 426
Combining functions with a graph 427
Custom streams 427
There's more... 428
Bridging core.async 430
Getting ready 430
Memories - reviewing core.async basics 431
Pub/sub 432
How to do it... 433
Async socket server 433
Async socket client 434
Chiming 435
There's more... 436
Client to Python WebSockets 436
Clojure WebSocket server 438
On Quasar/Pulsar 440
Getting ready 440
How to do it... 440
How it works... 445
Watching over other actors lifecycles 446
State of an actor 447
There's more... 447
Blazar 448
## 7. Advanced Tips 450
Introduction 450
Hacking the Clojure code 451
Getting ready 451
How to do it... 452
How it works... 454
There's more... 456
Using Reader Conditionals, compile to Clojure, and ClojureScript 459
Getting ready 459
How to do it... 462
Your first Reader Conditional 462
Using Reader Conditionals in namespaces 463
How it works... 464
The form of Reader Conditionals and Splice macros 464
Macros in ClojureScript 466

Introduction 510

Clojure with Vaadin - easy web widgets 511

Getting ready 511

Project settings 511

The Clojure/Vaadin flow 512

How to do it... 515

Using the Calendar widget 515

Reacting to events 517

Clicking a button 518

Using and reloading namespaces 519

Server-side push 521

There's more... 524

Reactive Vaadin - ideas 524

Reactive Vaadin - CPU monitoring 526

Reactive Vaadin - Telegram on Raspberry Pi 528

Deployment bonus 530

Quickly create a REST API with Liberator 531

Getting ready 531

How to do it... 532

Your first Liberator 532

Using defresource 534

Parameterized resources 535

Defining GET/POST/PUT/DELETE methods for REST APIs 536

How it works... 540

Methods and status codes of HTTP 540

Tracing requests 541

There's more... 546

Persistent REST resources using clojure.java.jdbc 546

Preparing for jdbc 546

Our service spec 547

Creating a product table and defining access functions 548
