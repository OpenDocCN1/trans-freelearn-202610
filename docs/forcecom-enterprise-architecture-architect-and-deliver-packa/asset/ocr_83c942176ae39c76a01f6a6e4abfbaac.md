# # Encapsulating an object's behavior in code

Apex Triggers present a way to write code to implement additional behavior for Custom Objects (or Standard Objects for that matter). They provide a place to implement more complex behavior associated with database events, such as **create**, **update**, and **delete**.

However, as an Apex Trigger cannot have `public` methods, it does not provide a means to write code that logically belongs with the object but is not necessarily linked with a database event. An example of this would be logic to calculate **Contestants** championship points or checking compliance against the many *Formula1 rules and regulations*, which we will look at later in this chapter. Furthermore, they do not present the opportunity to leverage **object-oriented programming** (OOP) approaches, also discussed later in this chapter.

As a result, developers often end up putting this logic elsewhere, sometimes in a controller or helper class of some kind. Thus, the code implementing the behavior for a given object becomes fragmented and there is a breakdown of *Separation of Concerns*.

Lifting your Apex Trigger code out of the body of the trigger and into an **Apex Class**, perhaps named by some convention based on the object name, is a good start to mitigating this fragmentation, for example `RaceTriggerManager` or `RaceTriggerHandler`. Using such approaches, it then becomes possible to create other methods and break up the code further. These approaches are only the start in seeking a more complete interpretation of the domain model concept described in this chapter.

Using the `Manager` or `Handler` approach tends to attract logic relating to the Apex Trigger event model and nothing else, leaving other object-related logic to find less obvious locations in the code base.

# # Interpreting the Domain layer in Force.com

In this chapter, we look at the Force.com interpretation of the Domain layer pattern, one which ensures behavior invoked both through Apex Trigger events and through direct Apex method calls (from the Service layer and other Domain classes) are considered.