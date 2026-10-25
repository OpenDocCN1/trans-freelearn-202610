'Robert Downey Jr.', 'Will Smith',
'Robert De Niro', 'Christian Bale',
'Brad Pitt','Tom Hanks',
'Robert Pattinson', 'Ben Stiller'];

Now when initializing the ListView widget, provide the preceding elements as a dataSource
and also include a filterable attribute and set its value to true:

$('#listViewContainer').kendoMobileListView({
dataSource: listElements,
filterable: true
});

When you set the filterable attribute to true, a search input field is added to the top
of the list. When the user starts to type in the search input field, the list is filtered to show the
options that match the user input. Here, the user input string is compared with elements in
the list and only those that start with the input string are displayed in the list. If you would like
to change this logic so that it returns those elements that contain the input string instead of
the ones that start with it, then mention the operator field as follows:

$('#listViewContainer').kendoMobileListView({
dataSource: listElements,
filterable: {
"operator": "contains"
}
});

The operator field can contain one of these values: startsWith, endswith, contains,
eq (equal to), neq (not equal to), lt (less than), lte (less than or equal to), gt (greater than),
and gte (greater than or equal to).

When working with dataSource, which contains objects, the widget would fail to filter the list
because it wouldn't know the field against which the user input has to be matched. To fix this,
you can specify the field option:

$('#listViewContainer').kendoMobileListView({
dataSource: actors,
template: '#= name #',
filterable: {
"field": "name",
"operator": "contains"
}
});