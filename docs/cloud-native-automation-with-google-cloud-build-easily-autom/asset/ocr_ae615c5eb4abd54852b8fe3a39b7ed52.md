Consistent with the first version, deploying Cloud Functions 2nd gen can also be done with a single command:

```
$ gcloud functions deploy function-name --gen2 --source .
```

There are multiple improvements for this re-platforming under the hood:

+   - Longer code execution timeouts
+   - Support for concurrent requests to a single instance of code
+   - Support for traffic splitting between revisions of code
+   - Support for larger instance types

Cloud Functions 2nd gen also integrates with Google Cloud's Eventarc (https://cloud.google.com/eventarc/docs), which is a managed event-delivery service with dozens of out-of-the-box event supports from Google Cloud resources.

# Comparing Cloud Functions and Cloud Run

Because Cloud Functions 2nd gen is built as an abstraction layer atop Cloud Run, it should be noted that there can be an overlap in the use case. While a full discussion of when to use one versus the other is out of the scope of this chapter, it should be noted that generally, Cloud Functions should typically perform a single task, performing some sort of processing in response to an event. In addition, while Cloud Functions 2nd gen does use Cloud Run under the hood, you cannot customize the containers it creates and deploys. Rather, you are subject to the provided runtimes and working within those constraints.

Cloud Run, on the other hand, is a full-fledged container-as-a-service platform. This means that you can build anything into a container image and run it on Cloud Run – from web services to monolithic applications!

With that, let's review how developers can simplify the production of the standard package – the container – that Cloud Run and Cloud Functions 2nd gen use. This simplification can help minimize overhead cognitively, usage of personal workstation resources, and local optimizations that lead to silos.

# Building containers without a build configuration

If the mission of building on serverless is to make running software as easy as possible for developers, it is important to consider areas where you can reduce cognitive overload – especially in areas that are not unique or specific to providing their business with unique value.