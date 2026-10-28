Save the file.

6. Open your form_example app’s views.py file. At the top of the file, add a line to import ExampleForm from your forms.py file:

```
from .forms import ExampleForm
```

7. Inside the form_example view, instantiate the ExampleForm class and assign it to the form variable:

```
form = ExampleForm()
```

8. Add the form variable to the context dictionary using the form key. The return line should look like this:

```
return render(request, "form-example.html",
    {"method": request.method, "form": form})
```

Save the file.

Make sure you haven’t removed the code that prints out the data the form has sent, as we will use it again later in this exercise.

9. Open the form-example.html file inside the form_example app’s templates directory. You can remove nearly all of the contents of the form element, except the {% csrf_token %} template tag and the submit buttons. When you’re done, it should look like this:

```
<form method="post">
    {% csrf_token %}

    <p>
        <input type="submit" name="submit_input"
               value="Submit Input">
    </p>
    <p>
        <button type="submit" name="button_element"
                value="Button Element">
            Button With <strong>Styled</strong> Text
        </button>
    </p>
</form>
```

10. Add a rendering of the form variable using the as_p method. Put this on the line after the {% csrf_token %} template tag. The whole form element should now look like this:

```
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <p>
```