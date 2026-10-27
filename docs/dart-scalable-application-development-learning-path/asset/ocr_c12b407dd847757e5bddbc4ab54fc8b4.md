MIME stands for Multipurpose Internet Mail Extensions. It is an IETF standard that is used to indicate the type of data that a file contains. This list is continually getting longer as more types of data are shared online. For more details, refer to http://www.iana.org/assignments/media-types/media-types.xhtml.

# Robots.txt

People with web browsers are only one type of web users. There are numerous bots and spiders out on the live Internet. To give some guidance to the nonhuman visitors, the robots.txt standard tells the bot whether they are welcome, and if it is a search engine, then how to index it. For the purpose of the blog server, we want to welcome search engines to index the content to bring in more readers.

The text content of the response for this will be:

```
User-agent: *
Disallow:
```

The robots.txt request needs to be handled and served in the same element as a regular text page:

```
void _serveRobotsFile(HttpRequest request) {
    request.response
    ..statusCode = HttpStatus.OK
    ..headers.set('Content-Type', 'text/html')
    ..write(RobotsTxt)
    ..close();
}
```

The page is served with a positive OK HTTP status.

# Rendering a single blog post

The blog posts will be served on a single page for the permanent links from Uris, such as http://127.0.0.1:8080/post6.html, and the _getContent method parses the Uri path to get the ID number of the post:

```
if (path.startsWith("/post")) {
    String idfromUrl =
        path.replaceFirst("/post", "").replaceFirst(".html", "");
    int id = int.parse(idfromUrl);
    return hostedBlog.getBlogPost(id).HTML;
}
```