In the preceding code snippet, the transport object defines endpoint URLs for various actions,
such as read, create, update, and delete. The create, update, and delete actions
specify the endpoint URL as well as the http method to use when sending a request. The
http method is required if you have a RESTful service that accepts the request and invokes
the correct resource by examining the http method in the request. If this is not specified,
then a GET request is sent by default.

# How it works...

After setting the editable attribute to inline and adding its command configuration in
columns, each row in the Grid has two buttons, that is, Edit and Delete, as shown in the
following screenshot:

| Movie | Year | Rating | |
| --- | --- | --- | --- |
| The Shawshank Redemption | 1994 | 9.2 | Edit Delete |
| The Godfather | 1972 | 9.2 | Edit Delete |
| The Godfather - Part 2 | 1974 | 9 | Edit Delete |
| Pulp Fiction | 1994 | 8.9 | Edit Delete |
| The Good, the Bad and the Ugly | 1966 | 8.9 | Edit Delete |

When you click on Edit, the selected row switches to the edit mode and text-input fields
are shown.

| Movie | Year | Rating | |
| --- | --- | --- | --- |
| The Shawshank Redemption | 1994 | 9.2 | Edit Delete |
| The Godfather | 1972 | 9.2 | Edit Delete |
| The Godfather - Part 2 | 1,974.00 | 9.00 | Update Cancel |
| Pulp Fiction | 1994 | 8.9 | Edit Delete |
| The Good, the Bad and the Ugly | 1966 | 8.9 | Edit Delete |