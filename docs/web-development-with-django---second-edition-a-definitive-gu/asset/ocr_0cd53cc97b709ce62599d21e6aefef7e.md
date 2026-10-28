# Template tags

A tag is similar to a programmatic control flow, such as an if condition or a for loop. A tag is represented between two curly braces and percentage signs, as shown. Here is an example of a for loop iterating over a list using template tags:

```
{% for element in element_list %}

{% endfor %}
```

Unlike Python programming, we also add the end of the control flow by adding the end tag, such as {% endfor %}. This can be used along with template variables to display the elements in the list, as shown here:

```
<ul>
    {% for element in element_list %}
        <li>{{ element.title }}</li>
    {% endfor %}
</ul>
```

# Comments

Comments in the Django template language can be written as shown here; anything in between {% comment %} and {% endcomment %} will be commented out:

```
{% comment %}
    <p>This text has been commented out</p>
{% endcomment %}
```

# Filters

Filters can be used to modify a variable to represent it in a different format. The syntax for a filter is a variable separated from the filter name using a pipe (|) symbol:

```
{{ variable|filter }}
```

Here are some examples of built-in filters:

+   - {{ variable|lower }}: This converts the variable string into lowercase
+   - {{ variable|title }}: This converts the first letter of every word into uppercase

Let’s use the concepts we have learned to develop the book review application.