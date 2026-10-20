```
</p>
</article>
{% else %}
    <section class="container-fluid">
        <h2 align="center">Todos</h2>
        <br>
        <div class="card">
            <ul class="list-group list-group-flush">
                {% for todo in todos %}
                    <li class="list-group-item">
                        {{ loop.index }}. <a href="/todo/{{loop.index}}"> {{
                        todo.item }} </a>
                    </li>
                {% endfor %}
            </ul>
        </div>
    {% endif %}
</section>
</main>
{% endblock %}
```

In the previous code block, the todo template is inheriting the homepage template. We also defined the todo_container block, whose content will be displayed in the parent template.

The todo template is used by both routes for retrieving all the todos and for a single todo. As a result, the template renders different content depending on the route used.

In the template, Jinja checks to see whether a todo variable is passed using the {% if todo %} block. The todo detail is rendered if a todo variable is passed, otherwise, it renders the content in the {% else %} block, which is the list of todos.