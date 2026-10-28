This uses the map method to iterate over the array of books in state. We generate <li> for each book, using the book’s pk as the key instance for the list item. The content of <li> is the book’s title. We define a buttonText variable to store (and update) the text that the button will display. If we currently have a fetch operation running, then this will be Fetch in Progress. Otherwise, it will be Fetch. Finally, we return <div> that contains all the data we want. The content of <ul> is the bookListItems variable (the array of <li> instances). It also contains a <button> instance added in a similar way to the previous exercises. The onClick method calls the doFetch method of the class. We can use disabled to make the button disabled (that is, the user can’t click the button) if there is a fetch in progress. We set the button text to the buttonText variable we created earlier. You can now save and close react-example.js.

+   6. Open react-example.html. We need to replace the ClickCounter render (from Exercise 16.03 - React component properties) with a BookDisplay render. Delete the name and target variable definitions. We will instead render <BookDisplay>. Set the url property as a string and pass in the URL to the book list API, using the {% url %} template tag to generate it. The ReactDOM.render call should then look like this:

```
ReactDOM.render
  (<BookDisplay url="{% url 'api:book-list' %}" />,
    document.getElementById('react_container'));
```

You can now save and close react-example.html.

+   7. Start the Django dev server if it’s not already running, then visit http://127.0.0.1:8000/react-example/. You should see a single Fetch button on the page (Figure 16.16):

![](bbox=[0.115, 0.545, 0.865, 0.675])

Figure 16.16 – The book Fetch button

After clicking the Fetch button, it should become disabled and have its text changed to Fetch in Progress, as we can see here: