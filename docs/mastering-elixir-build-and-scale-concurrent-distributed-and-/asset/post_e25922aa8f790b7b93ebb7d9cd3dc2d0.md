
- Task 153
  - Parallel tasks 154
- Using (D)ETS 162
  - Disk-based ETS 172
- Registry 175
- Summary 180
## Chapter 5: Demand-Driven Processing 182
- GenStage 182
  - The upload pipeline 187
    - The notifier stage 189
    - The RemoteStorage stage 190
    - The Encryption stage 191
    - The Starter stage 192
  - The download pipeline 195
    - The Notifier stage 196
    - The Encryption stage 197
    - The RemoteStorage stage 199
    - The Starter stage 200
    - Final remarks 201
- Flow 203
  - Sample data 204
  - Lazy queries 210
  - Disk-usage rank query 213
- Summary 217
## Chapter 6: Metaprogramming – Code That Writes Itself 218
- The abstract syntax tree 218
- Tinkering with macros 222
  - Hygienic accesses 225
  - Famous (un)quotes 227
    - Macros – a silver bullet? 232
- Extending modules 233
  - Using the use and __using__/1 macros 247
- Creating a domain-specific language 252
  - Registering module attributes 253
  - Collecting a list of pipeline steps 256
  - Generating worker specs and subscription options 260
    - Producer stage 261
    - ProducerConsumer and Consumer stages 262
    - Collecting the pipeline worker specs 264
    - Defining the supervisor functions 267
  - Streamlining GenStage modules 268
    - Simpler pipeline producers 268
    - Simpler pipeline (producer) consumers 273
- Macros cheat sheet 276
- Summary 278
## Chapter 7: Persisting Data Using Ecto 279
- Connecting to the database 279
- Schemas and migrations 284
  - Schemas 284
    - Custom field types 286
    - Users or media owners? 287
    - Schema or not – that is the question 289
  - Migrations 290
  - Relationships 292
- Changesets 296
  - Media 297
  - Users 302
  - Media ownership 304
  - Constraints 307
- Queries 313
  - Media folder queries 315
  - Loading schemas and their associations 320
  - Queries with raw SQL 324
  - Finding top users with aggregates 326
- Summary 332
## Chapter 8: Phoenix – A Flying Web Framework 334
- The Plug specification 336
  - Creating a module plug 337
  - Creating a function plug 337
- Routing requests 339
- Handling requests in a controller 344
- Rendering views 351
  - Layouts 359
- Authenticating users 361
- Implementing a JSON API 371
  - Authenticating users in the API 376
- Interactive applications using channels 380
  - Preparing the server 382
  - Joining channels on the client 385
  - Exchanging events 387
  - Authenticating socket connections 394
  - Tracking users statuses using Presence 396
- Summary 401
## Chapter 9: Finding Zen through Testing 403
- Unit testing 404
  - Testing functions without side-effects 404
  - Testing functions with side-effects 410
    - Creating mocks with Mox 410
    - Testing interactions with the repository separately 415
- Testing the documentation 419
- Integration testing 421
- Testing Phoenix Channels 425
- Testing macros 428
- Property-based testing 430
- Summary 435
## Chapter 10: Deploying to the Cloud 437
- Releasing with Distillery 437
  - Configuring the release 439
  - Interpolating environment variables 442
  - Creating the release 445
  - Creating a custom release task 447
- Containerizing our application 450
  - Creating a development container 452
  - Orchestrating more than one container 454
  - Composing the deployment containers 459
- Deploying to Kubernetes 462
  - Configuring the cloud database 466
  - Creating a namespace 468
  - Creating secrets 469
  - Publishing the production image 470
  - Deploying your first pod 471
  - Creating a Kubernetes job 475
  - Exposing your pods to the world 477
- Continuous deployment with Travis CI 479
- Connecting the deployed Elixir nodes 485
  - Testing the connected nodes 491
- Summary 492
## Chapter 11: Keeping an Eye on Your Processes 494
- Collecting metrics 494
  - Exposing Prometheus metrics 496
  - Creating custom Prometheus metrics 498
  - Local Prometheus server 502
  - Deploying Prometheus in Kubernetes 505
  - Calculating percentiles 516
  - Setting Grafana variables 517
- A window to your nodes 523
  - Connecting to a containerized node 524
  - Connecting to an Erlang node running in Kubernetes 528
  - Using a remote shell 532
- Inspecting application behavior 535
  - Profiling with the fprof profiler 536
  - Tracing with the :dbg module 541
- Summary 545
## Table of Contents
- Other Books You May Enjoy 548
- Index 551
## Preface
Running scalable, concurrent, and fault-tolerant applications is a demanding endeavor. After learning the abstractions that Elixir offers, developers are able to build such applications effortlessly. That being said, there is a big gap between playing around with Elixir and running it in production and serving live requests. Mastering Elixir helps you to fill this very gap, and it not only goes into detail about how Elixir works, but also guides you to put the learned concepts to good use with the help of concrete examples.

In this book, you will learn how to build a rock solid application, beginning by using Mix to create a new project. You will then explore how to use Erlang’s OTP as well as the Elixir abstractions that are built on top of it, allowing you to build applications that are easy to parallelize and distribute. Having got to grips with the basics, you will master Supervisors and comprehend how they are the basis for building fault-tolerant applications. In addition to this, you will understand how to use Phoenix in order to create a web interface for your application.

Upon finishing implementation, you will study how to thoroughly test and monitor the developed file server. Toward the end, you will learn how to take this application to the cloud, using Kubernetes to automatically deploy, scale, and manage it.
## Who this book is for
Mastering Elixir is for you if you’re a programmer who has some experience in using Elixir and want to take it to the next level. This comprehensive guide shows you how to build, deploy, and maintain robust applications, allowing you to go from tinkering with Elixir on side projects to using it in a live environment. However, no prior knowledge of Elixir is required to enjoy the complex topics covered in the book.
## What this book covers
Chapter 1, *Preparing for the Journey Ahead*, starts our odyssey by introducing Elixir. It starts by covering the data types of Elixir and how pattern matching works, moving on to then explore how to create modules and functions, while also seeing, among other things, how to work with collections and use control flow in Elixir. After reading this chapter, you will be familiar with Elixir's syntax, and how to use it to write some simple functions.

Chapter 2, Innards of an Elixir Project, kicks off by analyzing what makes an Elixir application and what an Elixir project is. It then examines how to use the Mix tool to create projects and manage their dependencies. It's here that we'll create an umbrella project for the ElixirDrip application that will be developed throughout this book.

Chapter 3, Processes – The Bedrock for Concurrency and Fault-tolerance, starts by exploring how the BEAM VM works and its concurrency model, the actor model. Then, it shows how to create and work with processes in Elixir, building a piece of the ElixirDrip application as an example. This chapter also explores how to link and monitor processes, and how Supervisors build on top of this to detect crashing processes. Finally, this chapter explores how we can group Supervisors to create supervision trees, which enable the creation of fault-tolerant applications.

Chapter 4, Powered by Erlang/OTP, introduces OTP and examines both the battle-tested OTP abstractions that Elixir inherited from Erlang, such as GenServer and Erlang Term Storage, and the new Agent, Task and Registry abstractions that Elixir brought to life. Each abstraction is put to good use by implementing a media cache, a text search function, and a search results cache.

Chapter 5, Demand-Driven Processing, goes a long way to explain how the GenStage and Flow abstractions introduced by Elixir let the developer process data using a demand-driven paradigm. Instead of processing data as fast as possible to match the rate at which data is produced, this approach turns the problem on its head and forces the producer to inject new data into the pipeline at a rate controlled by the data consumers. It's in this chapter that the upload and download pipelines of the ElixirDrip application are developed.

Chapter 6, Metaprogramming–Code that Writes Itself, unveils the constructs that allow the developer to easily control what happens in compile time, by writing regular Elixir code that produces more code. The chapter starts by iterating on a macro whose purpose is to measure the time a function takes to execute, and ends by implementing a Domain-Specific language which significantly simplifies the media upload and download pipelines defined in the previous chapter.

Chapter 7, Persisting Data Using Ecto, explores how to work with databases. It starts by explaining how to connect an application to a database and how database structure changes can be applied as migrations. It then examines how to create and enforce table relations and how to use changesets to persist and update data. In the end, different ways to query the database are analyzed.
