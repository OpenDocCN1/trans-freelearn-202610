

## 媒体画廊

一旦我们确定了要操作的文件夹，就需要检查该文件夹是否确实存在于文件系统中。如果不存在，我们可能需要创建它。

```csharp
//make sure the directory is ready for use
saveToFolder += DateTime.Now.Year.ToString() +
                DateTime.Now.Month.ToString() + "/";
if (!Directory.Exists(Server.MapPath(saveToFolder)))
    Directory.CreateDirectory(Server.MapPath(saveToFolder));
```

接着，我们需要设置一些其他变量。我们需要获取当前操作的 Account（账户）。我们还需要接收上传的文件。最后，我们需要获取要保存文件的文件夹的完整路径。

```csharp
Account account = _accountService.GetAccountByID(
    _webContext.AccountID);
HttpFileCollection uploadedFiles = Request.Files;
string Path = Server.MapPath(saveToFolder);
```

现在我们已经有了上传文件的集合，需要逐个处理每个文件。为此，我们将启动一个 for 循环。在每次迭代的开始，我们需要获取一个要处理的文件。

```csharp
for(int i = 0 ; i < uploadedFiles.Count ; i++)
{
    HttpPostedFile F = uploadedFiles[i];
}
```

接下来，我们需要初始化更多变量，以确定文件的存储位置及存储方式。我们将从查询字符串中获取文件夹 ID。请注意，我们目前假设处理的是静态 fileType 为 1 的图像文件。如果我们创建另一个页面来上传音频文件，则应该传入 fileType。然后，我们通过解析上传文件名的末尾来获取 uploadedFileName。得到文件名后，我们获取文件扩展名。由于我们要将文件保存到文件系统，如果不确保文件名唯一，就会有覆盖文件的风险。为此，我们创建一个新的 GUID 字符串作为文件系统中的文件名。有了这些数据，我们就足以构建最终的文件名。然后我们创建域对象文件。

```csharp
string folderID = _webContext.AlbumID.ToString();
string fileType = "1";
string uploadedFileName =
    F.FileName.Substring(F.FileName.LastIndexOf("\\") + 1);
string extension =
    uploadedFileName.Substring(uploadedFileName.LastIndexOf(".") + 1);
```

```csharp
break;
case "swf":
    file.FileTypeID = (int)File.Types.SWF;
    break;
default:
    goodFile = false;
    break;
}
break;
case "3":
    file.FileSystemFolderID = (int)FileSystemFolder.Paths.Audios;
    switch (extension.ToLower())
    {
        case "wav":
            file.FileTypeID = (int)File.Types.WAV;
            break;
        case "mp3":
            file.FileTypeID = (int)File.Types.MP3;
            break;
        case "flv":
            file.FileTypeID = (int)File.Types.FLV;
            break;
        default:
            goodFile = false;
            break;
    }
    break;
}
```

接下来，我们尝试为域 **File** 对象填充所有属性，例如上传文件的大小、所属账户、文件系统名称等。

```csharp
file.Size = F.ContentLength;
file.AccountID = account.AccountID;
file.DefaultFolderID = Convert.ToInt32(folderID);
file.FileName = uploadedFileName;
file.FileSystemName = guidName;
file.Description = "";
file.IsPublicResource = false;
```

现在我们可以开始实际工作了。如果 goodFile 标志仍为 true，我们可以将文件对象提交到数据库。然后，我们将实际上传的文件以其新文件名保存到文件系统。最后，如果 fileType 是图片，我们会使用 Resize() 方法对文件进行处理，以生成我们需要的各种尺寸的文件。

```csharp
if (goodFile)
{
    _fileService.SaveFile(file);
    F.SaveAs(fullFileName);
    if(Convert.ToInt32(fileType) == ((int)Folder.Types.Photo))
    {
        Resize(F,saveToFolder,guidName,extension);
    }
}
```

要进入 Resize() 方法，我们必须传入正在处理的上传文件、要将文件保存到的文件夹、为文件系统名称生成的 GUID，以及上传文件的扩展名。进入 Resize() 方法后，我们可以为上传的照片创建所需的所有尺寸变体。我们首先设置一个 foreach 循环，该循环将遍历类级 sizesToMake 集合中的字典。

```csharp
public void Resize(HttpPostedFile F, string SaveToFolder, Guid
                     SystemFileNamePrefix, string Extension)
{
    //Makes all the different sizes in the sizesToMake collection
    foreach (KeyValuePair<string, int> pair in sizesToMake)
    {

using(System.Drawing.Image image =
        System.Drawing.Image.FromStream(F.InputStream))
//determine the thumbnail sizes
using(Bitmap bitmap = new Bitmap(image))
{
    decimal Ratio;
    if(bitmap.Width > bitmap.Height)
    {
        Ratio = (decimal) pair.Value / bitmap.Width;
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

确定尺寸后，我们可以调整其大小并保存到文件系统。为此，我们再次引用上传的文件并重建一个新的 Bitmap。然后，就可以将新尺寸的 bitmap 保存到文件系统。但请注意，在这种情况下，当我们保存文件到文件系统时，并没有使用字典中的键名来为文件名添加片段。这会导致文件名由 {GUID}__{key}-{extension} 组成。这意味着每个文件都是唯一的，并且会拥有各种尺寸且名称唯一的文件。

```csharp
using(System.Drawing.Image image =
        System.Drawing.Image.FromStream(F.InputStream))
using(Bitmap bitmap = new Bitmap(image, NewWidth, NewHeight))
{
    bitmap.Save(Server.MapPath(SaveToFolder + "/" +
        SystemFileNamePrefix.ToString() + "__"
        + pair.Key + "-" + Extension),
        image.RawFormat);
}
```

我们继续遍历字典中的所有不同文件尺寸，为每个尺寸创建新文件。完成后，我们将处理下一个上传的文件。现在，让我们编写一个测试页面来测试我们的接收页面。

### 使用 Flash 上传器接收文件

既然我们已经有了接收上传文件的方法，接下来让我们创建用于上传少量文件的 UI。这是一个基于 Flash 的 UI。你可以下载 Flash 试用版，也可以寻找 Swish 或 Flex 等产品中的替代方案。

由于本书并非专门讲解 Flash，我们只快速浏览一下这个主题。我们将创建一个简单的 Flash UI，其中包含一个浏览框、几个动态文本框和一些标签。我们将整个 UI 构建在一个帧上，UI 效果如下图所示：

![](img/15a62e134135dde036b0ac3cbb01dab6_255_0.png)

完成 UI 后，我们需要将 Flash 上传器嵌入到页面中。为此，我们需要在承载 Flash 上传器的页面中添加以下代码。

```aspx
//Fisharoo/Web/Photos/AddPhotos.aspx
<object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000"
codebase="http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=8,0,0,0"
    width="550" height="510" id="FileUpload">
    <param name="allowScriptAccess" value="sameDomain" />
    <param name="movie" value="../Files/FileUpload.swf?SiteRoot=<%Response.Write(ctx.RootUrl);%>&AlbumID=<%Response.Write(ctx.AlbumID); %>&FileType=<%Response.Write(ctx.FolderType);%>&AccountID=<%Response.Write(ctx.AccountID); %>" />
    <param name="quality" value="high" />
    <param name="bgcolor" value="white" />
    <embed src="../Files/FileUpload.swf?SiteRoot=<%Response.Write(ctx.RootUrl);%>&AlbumID=<%Response.Write(ctx.AlbumID); %>&FileType=<%Response.Write(ctx.FolderType);%>&AccountID=<%Response.Write(ctx.AccountID); %>"
        quality="high" bgcolor="#ffffff" width="550" height="220"
        name="FileUpload" align="middle"
        allowscriptaccess="sameDomain" type="application/x-shockwave-flash" pluginspage="http://www.macromedia.com/go/getflashplayer" />
</object>
```

一旦页面显示了 UI 并插入了相应的变量，就可以正常使用了。与 Flash UI 类似，我们还将提供一个 Silverlight UI，如前所述，这是我们从 codeplex 网站获取的一个控件。

