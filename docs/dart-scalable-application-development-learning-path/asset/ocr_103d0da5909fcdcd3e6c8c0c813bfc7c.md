```
```csharp
initBlog() async {
    _cache = {};
    IDs = new List<int>();
    postPaths = new Map<int, String>();
    imgPaths = new Map<int, String>();
    await _loadPostList();
    _loadImgList();
}
```
```

The BlogServerApp and _performNewPost methods perform the task of calling initBlog before serving the new post, as follows:

```
```csharp
hostedBlog.initBlog();

req.response
  ..statusCode = HttpStatus.OK
  ..headers.set('Content-Type', 'text/html')
  ..redirect(new Uri(path: "post$newID.html"))
  ..close();
```
```

Once the blog is reinitialized, the application serves the new blog post to the user as a single page.

# Caching

The serving of content in the first iteration of the web server was rather inefficient—I hope you noticed! On every GET request, the blog source file is read from the disk and is processed. To gain some efficiency, we can use a map added to the Blog class as a field. Let's take a look at the following code snippet for more information:

```
```csharp
BlogPost getBlogPost(int index) {
  if (!_cache.containsKey(index)) {
    _cache[index] = new BlogPost(postPaths[index], index);
  }
  return _cache[index];
}
```
```

The map object uses the blog index as a key and the resultant blog post as the value. This cache is kept in the memory so that if the blog server application is stopped and restarted, new requests will not initially come from the cache.