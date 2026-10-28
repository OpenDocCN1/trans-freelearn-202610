While a thumbnail this size might not be that useful, it at least lets us be sure that the image resize has worked correctly.

In this exercise, we changed FileField on UploadForm to ImageField. We saw that the browser wouldn’t let us upload anything other than images. We then added code to the media_example view to resize the uploaded image using PIL.

We have encouraged the use of a separate web server to serve static and media files for performance reasons. However, in some cases, you might want to use Django to serve files, for example, to provide authentication before allowing access. In the next section, we will discuss how to use Django to serve media files.

# Serving uploaded (and other) files using Django

Throughout this chapter and Chapter 5, Serving Static Files, we have discouraged serving files using Django. This is because it would needlessly tie up a Python process just serving a file – something that the webserver is capable of handling. Unfortunately, web servers do not usually provide dynamic access control, that is, allowing only authenticated users to download a file. Depending on your web server used in production, you might be able to have it authenticate against Django and then serve the file itself; however, the specific configuration of specific web servers is outside the scope of this book.

One approach you can take is to specify a subdirectory of your MEDIA_ROOT directory and have your web server prevent access to just this specific folder. Any protected media should be stored inside it. If you do this, only Django will be able to read the files inside. For example, your web server could serve everything in the MEDIA_ROOT directory except for a MEDIA_ROOT/protected directory.

Another approach would be to configure a Django view to serve a specific file from disk. The view will determine the path of the file on disk to send, then send it using the FileResponse class. The FileResponse class takes an open file handle as an argument and tries to determine the correct content type from the file’s content. Django will close the file handle after the request completes.

The view function will accept the request and a relative path to the file to be downloaded as parameters. This relative path is the path inside the MEDIA_ROOT/protected folder.

In our case, we will just check whether the user is anonymous (not logged in). We’ll do this by checking the request.user.is_anonymous property. If they aren’t logged in, then we will raise a django.core.exceptions.PermissionDenied exception, which returns an HTTP 403 Forbidden response to the browser. This will stop the execution of the view and not return any file:

```
import os.path
from django.conf import settings
from django.http import FileResponse
from django.core.exceptions import PermissionDenied
```