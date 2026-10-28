```
```csharp
using (MemoryStream ms = new MemoryStream(imageBytes, 0,
                                          imageBytes.Length))
{
    ms.Write(imageBytes, 0, imageBytes.Length);
    System.Drawing.Image img =
        System.Drawing.Image.FromStream(ms, true);
    Bitmap bmpCropped = new Bitmap(200, 200);
    Graphics g = Graphics.FromImage(bmpCropped);
    Rectangle rectDestination = new Rectangle(0, 0,
                              bmpCropped.Width, bmpCropped.Height);
    Rectangle rectCropArea = new Rectangle(X,Y,Width,Height);
    g.DrawImage(img, rectDestination, rectCropArea,
                GraphicsUnit.Pixel);
    g.Dispose();
    MemoryStream stream = new MemoryStream();
    bmpCropped.Save(stream,
                    System.Drawing.Imaging.ImageFormat.Jpeg);
    Byte[] bytes = stream.ToArray();
    profile.Avatar = bytes;
    _profileRepository.SaveProfile(profile);
}
_view.ShowApprovePanel();
```
```

This method expects the X and Y coordinates of where the crop is to start as well as the width and height of the crop. This gives us the location of the square that we plan to extract from our uploaded image.

We then load the image that is currently stored in the profile into the `imageBytes` byte array. Once we have the data stored in the array, we load it into the memory. Note that we perform this task inside a `using` statement. This ensures that all the resources are released once we complete our operation.

We then load the memory stream into an Image. Now, we create a new Bitmap object, bmpCropped with the size of the avatar that we wish to achieve. This will store our completed avatar. We then create a Graphics object, g, that will actually carry out the cropping on the bmpCropped Bitmap. We are now ready to actually perform the surgery on our image! We will start by creating two rectangles—one rectangle to hold our final image, and the other to hold the image to be cropped from our original image. We then call the DrawImage() method on our Graphics object and pass in our Image, the destination coordinates, the crop coordinates, and the unit of measure—pixels in this case. This results in the bmpCropped image having the appropriately cropped portion of our originally uploaded image.

Now, we pass that new image back into memory. We then convert it into a byte array, and finally save it back to our profile.