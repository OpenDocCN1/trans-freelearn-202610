
This example is pretty simple: it downloads a file from GitHub (the HTML page for this book's code package, actually), and saves it to the device's local filesystem as `index.html`. The name of the file could be anything you like; it doesn't have to match the original name (although you should endeavor to keep the same extension). Furthermore, it could be saved to a subdirectory; if the directory doesn't exist, it will be created automatically.

In the case of an error, the `downloadError` function is called. Our error handling is really pretty simple; it just logs out the `error` object. In production, you'd want to do quite a bit more to figure out what the problem was and whether there was anything you could do to work around it (or display a friendly error message to the user). The `err` object that is passed contains the following information that may be of use to you:

*   `code` indicates the error code. This can be one of the following:

    *   `FileTransferError.FILE_NOT_FOUND_ERR`: The file couldn't be saved at that download location.
    *   `FileTransferError.INVALID_URL_ERR`: The source URL was invalid (did you forget to use `encodeURI`?).
    *   `FileTransferError.CONNECTION_ERR`: A connection couldn't be established. Is there a network connection? Is the host reachable? If you're using SSL, make sure the certificates aren't expired and that they are signed by a trusted certificate authority.
    *   `FileTransferError.ABORT_ERR`: The transfer was aborted by the app.
    *   `FileTransferError.NOT_MODIFIED_ERR`: The source hasn't been modified since your last request. This occurs if an HTTP 304 code is returned for a request.

*   `source` is the source URL that the plugin tried to download.
*   `target` is the download destination.
*   `http_status` indicates the response code from the server. This could be just about anything. But in general, the following are the typical codes:

    *   `200` (`OK`): The request is completed successfully. At this point, the error occurred while saving the file, not at the server.
    *   `304` (`Not Modified`): The request has completed successfully, but the document hasn't been changed since the last time you requested it.
    *   `401` (`Not Authorized`): The request failed to complete, possibly because you aren't authenticated to the server.
    *   `403` (`Forbidden`): The request failed because you don't have access to the resource. You might want to ask the user for their credentials.
    *   `404` (`Not Found`): The request failed because the resource wasn't found.
    *   `500` (`Internal Server Error`): Something went very wrong on the server side. The error message may or may not have additional useful information.

*   `body` is the response from the server. If you receive a status other than 200, you might want to check here for more information.

As you can see, there are a lot of opportunities for error handling here. At a minimum, you will want to display a notice to the user that you couldn't download the file; but you can use the information present in the passed `error` object to tailor the message to the user.

If the download completes successfully, the `downloadSuccess` function is called. Again, our handler is really simple here; it just logs the `file` entry (that represents where the file was stored on the filesystem). At this point in a production app, you would continue to work with the file; when this method is called, you're guaranteed to have the file on the file system. If you need to use the File API for additional processing, you can do so by using the passed `file` entry.

If you need to provide credentials for protected resources, you can do so as follows:

```javascript
let fileTransfer = new FileTransfer();
let username = "testuser";
let password = "password";
let options = {
    headers: {
        "Authorization": `Basic ${btoa(`${username}:${password}`)}`
    }
}
fileTransfer.download(assetURL, downloadLocation, downloadSuccess, downloadError, false, options);
```

### Tip

If you're sending credentials, make sure you're using SSL to secure the communication.

## Uploading Files

Before we can upload a file to a server, we need to have something on the server that can receive and process the file. At a minimum, this process writes the file to a specific location on the server, but there's usually a little bit more going on. Usually, the request is checked to make sure something fishy isn't going on (such as a file is too large or of the wrong type). Then, the file is almost always renamed to prevent naming collisions. After this, it's up to you: you could run a virus scan on the file, resize it (if it's an image), upload it to a backend database, send it to another server, and so on.

### Note

The snippets in this section are located at `snippets/09/ex2-uploading-files` in the code package of this book. When using the interactive snippet playground, select **9: Transferring Files** and Example **2**.

The File Transfer plugin we've been using so far uses a multipart HTTP `POST` request to upload data, which is very useful. It means that we can use the same method we'd use to request files from users on a website, which typically involves an `<input type="file" />` widget on an HTML form and a PHP script that processes the results. In our case, we don't need the HTML form; our app is the one doing the uploading, not the user, so we can call the PHP script directly.

### Note

If you've already got a system like this in place, you can skip this section. Also, note that, although we use PHP in this section, the concepts can be applied to any server-side language.

Let's start building our PHP script. We'll call it `upload.php`. At the top, we're going to place some configuration settings that we'll use later on:

```php
<?php
$maximumFileSize = 5 * 1024 * 1024;
$uploadLocation = "../upload/";
$validMimeTypes = array("text/plain", "text/html", "image/png", "image/jpeg");
```

Although you may limit the maximum file size on the server in your `php.ini` settings, it's a good idea never to assume that these settings are what you want. As such, it's a good idea to check the file size in your upload script as well. One thing to remember, though, is that whatever size is specified here in `$maximumFileSize` won't override your `php.ini` settings, so you'll want to make sure the `post_max_size` and `upload_max_filesize` settings are large enough to accept a file of `$maximumFileSize` bytes.

The upload location is entirely up to you and your server configuration. The valid mime types are also up to you; these will let you filter out the files that aren't in the expected format. Of course, since our app is going to be the client that is uploading files, ideally, the app would never send a file of the wrong type, but it never hurts to check. Furthermore, your app may not be the only client for the upload script, so you will need to handle situations where the app may not be the one sending you data.

PHP automatically places information about the file being uploaded in a global variable called `$_FILES`. We can use this to extract information for our purposes:

```php
$uploadedFile = $_FILES['file'];
$uploadedFileError = $uploadedFile['error'];
$uploadedFileSize = $uploadedFile['size'];
$uploadedFileType = $uploadedFile['type'];
$uploadedFilePath = $uploadedFile['tmp_name'];
```

One important thing to note is that the `file` index on `$_FILES` is the file key. If you had an HTML form, it would match the `name` of your file input element. We'll call this the "file key":

```html
<form enctype="multipart/form-data" method="POST">
    <input type="file" name="file" />
</form>
```

Next, we're going to wrap everything we do in a `try…catch` statement. This will let us return an appropriate error if something goes wrong (otherwise we might return 200 even in an error condition):
