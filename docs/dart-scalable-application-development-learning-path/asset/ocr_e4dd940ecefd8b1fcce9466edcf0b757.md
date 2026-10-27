# Reading a folder of files

In the project, the posts subfolder of the content folder contains all the blog content. To obtain a list of all the files to load, the `Directory` object can be used to obtain the list, which can be processed with `forEach`:

```
void _loadPostList() {
    Directory blogContent = new Directory(_pathPosts);
    blogContent.list().forEach((File f) {
        String postFilename =
            path.basename(f.path).replaceAll(".txt", "");
        int id = int.parse(postFilename);
        IDs.add(id);
        postPaths[id] = f.path;
    }).then((v) {
        IDs.sort();
        IDs = IDs.reversed.toList();
    });
}
```

The file paths to the blog posts are stored in `Map`, so that they can be retrieved by the `id` value. `IDs` are stored in a list that can then be ordered (there is no guarantee that the filesystem will supply the list of filenames in any particular order), and so that we can easily retrieve posts with the most recently published post first.

# Request handling

The incoming `HttpRequest` object contains a wealth of information about the client—the request made and the network connection to the server:

```
handleRequest(HttpRequest request) {

    if (request.uri.path.endsWith(".html")) {
        _serveTextFile(request);
    }
    else if (request.uri.path.endsWith(".png")) {
        _servePngFile(request);
    }
    else if (request.uri.path == "/robots.txt") {
        _serveRobotsFile(request);
    }
    else {
        _serve404(request);
    }
}
```