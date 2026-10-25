# # Creating a two-way binding between View elements and JavaScript objects using the Model View ViewModel (MVVM) pattern

There are various architectural patterns that are used in most of the applications. The three very important architectural patterns are as follows:

+   - Model View Controller (MVC)
+   - Model View Presenter (MVP)
+   - Model View ViewModel (MVVM)

These patterns are heavily used in structuring both the desktop and server-side applications. In recent years, these patterns have been applied to client-side applications as well. Frameworks such as AngularJS, BackboneJS, and EmberJS provide components that can be readily used to build applications that follow one of these patterns. All three patterns mentioned previously try to separate the development of the user interface from the development of the business logic.

The **Model View ViewModel (MVVM)** pattern is an architectural pattern, which is based on MVC and MVP. Here, the model layer represents the domain-specific data. An example application will include the bank account number, the holder's name, the balance amount, and so on. The model layer represents raw data that needs to be formatted to be consumed by `View`. The `View` layer consumes the data provided by the model and formats it as required. On the other hand, the behavior of the application or the business logic is encapsulated in a layer called `ViewModel`.

There are several frameworks, such as KnockoutJS, KnockbackJS, and Kendo UI, that implement the MVVM pattern. In Kendo UI, the MVVM pattern is at the core of the application framework. The framework provides APIs using which the various components, such as model, `ViewModel`, and data binding (that binds the markup to the `ViewModel` object) can be accomplished.

# ## How to do it...

To illustrate how the MVVM pattern is implemented in Kendo UI, let's take a look at a very simple example in which the form elements are bound to a `ViewModel` object. The `ViewModel` object, in this case, would define the model data and also the behavior for one of the elements:

```
```
<form id="testView">
  Firstname: <input id="firstName" type="text"
```
```