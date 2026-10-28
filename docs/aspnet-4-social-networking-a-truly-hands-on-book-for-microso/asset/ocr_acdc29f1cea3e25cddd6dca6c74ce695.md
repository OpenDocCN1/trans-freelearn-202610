# Blogs

```
```csharp
int count = dc.Blogs.Where(b => b.PageName == blog.
PageName && b.AccountID == blog.AccountID).Count();
if (count > 0)
    result = false;
}
return result;
}
```
```

This method looks at all the blog entries except itself to determine if there are other blog posts with the same page name that are also by the same Account. This allows us to effectively lock down our users from creating duplicate page names. This will be important when we start to discuss our user-friendly URLs.

Next, we will look at a private method that will help us clean up these page name inputs. Keep in mind that these page names will be displayed in the browser's address bar and therefore need not have any characters in them that the browser would want to encode. While we can decode the URL easily this conversation is more about keeping the URL user-friendly so that the user and the search engine spiders can easily read where they are at. When we have characters in the URL that are encoded, we will end up with something like %20 where %20 is equivalent to a space. But to read my%20blog%20post is not that easy. It is much easier to read my-blog-post. So we will strip out all of our so called special characters and replace all spaces with hyphens. This method will be the CleanPageName() method.

```
```csharp
private Blog CleanPageName(Blog blog)
{
    blog.PageName = blog.PageName.Replace(" ", "-").Replace("!", "")
        .Replace("&", "").Replace("?", "").Replace(",", "");
    return blog;
}
```
```

You can add to this as many filters as you like. For the time being we are replacing the handful of special characters that we have just seen in the code.

Next, we will get into the service layers that we will use to handle our interactions with the system. Unlike other chapters this chapter will not introduce any new service layers — we will add to the existing one.

# Implementing the services/application layer

Once all the repositories are built for single serving purposes we can begin creating the services layer. We will not be creating any new services for this component but will need to add to the existing AlertService residing under //Fisharoo/BusinessLogic.