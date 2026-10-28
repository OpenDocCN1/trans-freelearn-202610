# Media Galleries

```
NewWidth = pair.Value;
decimal Temp = bitmap.Height * Ratio;
NewHeight = (int)Temp;
}
else
{
    Ratio = (decimal) pair.Value / bitmap.Height;
    NewHeight = pair.Value;
    decimal Temp = bitmap.Width * Ratio;
    NewWidth = (int)Temp;
}
}
```

Once we have our sizes determined we can resize it and save it to the file system. We do this by again setting a reference to the uploaded file and reconstructing a new Bitmap. With this in hand we can then save the bitmap to the file system with its new dimensions. Do note though that in this case when we save the file to the file system, we are not adding a section to the name using the key name from the dictionary. This results in a file name that is made up of {GUID}__{key}-{extension}. This means that every file will be unique and will have uniquely named files of various sizes.

```
using(System.Drawing.Image image =
        System.Drawing.Image.FromStream(F_InputStream))
using(Bitmap bitmap = new Bitmap(image, NewWidth, NewHeight))
{
    bitmap.Save(Server.MapPath(SaveToFolder + "/" +
        SystemFileNamePrefix.ToString() + "__"
        + pair.Key + "-" + Extension),
        image.RawFormat);
}
```

We continue to loop through all the different file sizes in our dictionary creating new files for each one. Once complete we will move to the next file that was uploaded. Let's now write a test page to test our receiving page.

# Receiving files with Flash uploader

Now that we have a way of receiving uploaded files, let's create the UI to upload a handful of files. This is a Flash-based UI. You can either download a trial copy of Flash or look at alternatives in a product such as Swish or Flex.

As this is not so much a Flash book, we will skim over this topic quickly. We will create a simple Flash UI that has a browse box, a couple of dynamic text boxes, and some labels. We will build the entire UI on one frame and the UI looks something as shown in the following screenshot: