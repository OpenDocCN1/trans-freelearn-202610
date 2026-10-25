

Here, the `#: <literal> #` expression is used. Rendering HTML-encoded values is particularly useful when you are accepting HTML strings as a part of a form input string, and they display the same in the result. The HTML characters/tags present in the data would be escaped and rendered as strings.

# How it works...

Now, let's understand what is happening in the preceding code examples. In the first example, the first line is as follows:

```
var template = kendo.template("Full Name: " + "<span> #= lastName # </span>," + "<span> #= firstName # </span>");
```

Here, the template function is used to compile the input string. Note that the input string contains HTML tags with hash-based expressions. This will return a function that can then be used with data to generate output. In the following four lines of the code snippet, a JavaScript data object with two variables, lastName and firstName, is created:

```
var data = {};
data.lastName = "Smith";
data.firstName = "Todd";
var result = template(data);
```

The last line in the preceding code snippet invokes the compiled function (assigned to the template variable), which then passes the JavaScript object as a parameter. This will replace the literal values inside the hash-based expression and generate the following output string:

Full Name: <span> Smith </span>, <span> Todd </span>

In the second example, the code is very similar, except that the `#: <literal> #` hash-based expression is used. This expression will escape the HTML tags and generate the output that contains HTML tags as it is.

# There's more...

As seen in the previous examples, the HTML is passed as a string to the kendo.template function. An alternative to this is to define a template in a script tag:

```
<script type="text/x-kendo-template" id="testTemplate">
    # for (var i=0; i < functions.length; i++) { #
        <li>#= functions[i] # </li>
    # } #
</script>
```

Note the hash-based syntax used in the preceding code as well; it contains a JavaScript for statement that iterates over an array and generates list tags (<li> tags). Now, to compile this template, we will refer to the script tag using the id attribute:

```
<script>
    var templateContent = $('#testTemplate').html();
    var template = kendo.template(templateContent);
    var functions = ["concat", "indexOf", "join", "lastIndexOf",
        "pop", "push", "reverse", "shift", "slice", "sort", "splice",
        "toString", "unshift", "valueOf"];
    var result = template(functions);
    $('ul.listContainer').append(result);
</script>
```

Here, the template is referred using the id attribute, #testTemplate. The content of the template is retrieved using $('#testTemplate').html(). The content is then used with the kendo.template function to generate the markup. This content is then inserted into the page by appending the same inside an unordered list using $('ul.listContainer').append(result).

> The script tag used here to write template has its type as text/x-kendo-template. This is a common way to implement templating on the client side. The browser simply ignores the script tag with a type that is not recognized by it.

# Creating a two-way binding between View elements and JavaScript objects using the Model View ViewModel (MVVM) pattern

There are various architectural patterns that are used in most of the applications. The three very important architectural patterns are as follows:

- Model View Controller (MVC)
- Model View Presenter (MVP)
- Model View ViewModel (MVVM)

These patterns are heavily used in structuring both the desktop and server-side applications. In recent years, these patterns have been applied to client-side applications as well. Frameworks such as AngularJS, BackboneJS, and EmberJS provide components that can be readily used to build applications that follow one of these patterns. All three patterns mentioned previously try to separate the development of the user interface from the development of the business logic.

The **Model View ViewModel (MVVM)** pattern is an architectural pattern, which is based on MVC and MVP. Here, the model layer represents the domain-specific data. An example application will include the bank account number, the holder's name, the balance amount, and so on. The model layer represents raw data that needs to be formatted to be consumed by `View`. The `View` layer consumes the data provided by the model and formats it as required. On the other hand, the behavior of the application or the business logic is encapsulated in a layer called `ViewModel`.

There are several frameworks, such as KnockoutJS, KnockbackJS, and Kendo UI, that implement the MVVM pattern. In Kendo UI, the MVVM pattern is at the core of the application framework. The framework provides APIs using which the various components, such as model, `ViewModel`, and data binding (that binds the markup to the `ViewModel` object) can be accomplished.

# How to do it...

To illustrate how the MVVM pattern is implemented in Kendo UI, let's take a look at a very simple example in which the form elements are bound to a `ViewModel` object. The `ViewModel` object, in this case, would define the model data and also the behavior for one of the elements:

```
<form id="testView">
  Firstname: <input id="firstName" type="text"
data-bind="value: firstName">
<br/>
Lastname: <input id="lastName"
type="text"
data-bind="value: lastName">
<br/>
Fullname: <input id="fullName"
type="text"
data-bind="value: fullName"
readonly>
<br/>
<input type="submit">
</form>
```

In the previous code snippet, a form with the id attribute, testview, that contains three input elements (textboxes) is created. Note the data-bind attribute in each one of these input elements. The data-bind attribute is used to specify the binding between View elements and the ViewModel object.

Here, the first input element specifies the value of the data-bind attribute as value: firstName. This indicates that the value of the input element is bound to the firstName model attribute. Now let's define ViewModel, which encapsulates the model data and behavior:

```
var viewModel = kendo.observable({
firstName: "Sagar",
lastName: "Ganatra",
fullName: function() {
return this.get("firstName") +
' ' + this.get("lastName");
}
});
```

A ViewModel object is created using the Observable interface on the kendo object. In this example, the ViewModel object contains the firstName, lastName, and fullName attributes. Note that while firstName and lastName are of the string type, the fullName attribute is a function that returns a string by concatenating firstName and lastName.

Now that we have the markup and ViewModel, they should be tied to each other. The bind method is used to bind View components to ViewModel:

```
kendo.bind($('form#testView'), viewModel);
```

The first argument to the bind function is the View component referred by using $('form#testView') and the second argument is the ViewModel object, viewModel. Once View and ViewModel are bound, any changes made to the model will update View, and any changes made by the user will be reflected in the model.

# How it works...

When you execute the previously mentioned code snippets, the form elements with the values mentioned in the model are populated, as shown in the following screenshot:

![](img/2a66507445090790d34dc5588b56b24c_25_0.png)

Notice that in the markup, the last input element for FullName is read only. It is bound to a fullName model attribute, which is a function that returns the string concatenated by firstName and lastName. When you change the value of either firstName or lastName, the fullName model also gets updated and the same is reflected in the View class.

![](img/2a66507445090790d34dc5588b56b24c_25_1.png)

In the preceding screenshot, when the value of the Firstname input element is changed, the value of the Fullname attribute also gets updated. Note that the model attributes are updated on a blur event on the View component.

# There's more...

In the previous screenshot, we saw how data binding can be used to set the values of input elements in the form. In addition to setting the value for some of the elements in the page, data binding can also be used to set attribute values (attr), to set HTML content (html), hide or show the elements in the page (visible and invisible), and so on.

