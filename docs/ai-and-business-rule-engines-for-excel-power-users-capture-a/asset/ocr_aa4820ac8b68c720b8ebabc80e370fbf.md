4. When Docker has started, you should see the home screen, as shown in **Figure 10.3**. If you don’t see this exact screen, try clicking on **Containers** in the top-left corner:

![](bbox=[0.147, 0.154, 0.873, 0.541])

Figure 10.3 – Docker Desktop home screen

5. If your screen is showing the green Docker whale icon (in the bottom-left corner) then congratulations, both Docker Desktop and the background engine (services) are running correctly.

While this isn’t a book about Docker, you’ll be spending some time on this screen to run examples. It’s worth taking a few moments to look around:

+   - The area at the bottom center of the screen is **Featured Images**, showing the range of containers that can be run within Docker. There are many other sources of images (notably Docker Hub and Quay from Red Hat) – we’ll use them to run the sample shortly.
+   - The **Images** (tab) is the list of available images that you can run. Images are read-only and never change. There are not likely to be many images on your computer at the moment; we’ll show you how to download some later in the chapter.
+   - The **Container** tab (on the top left) also allows you to return to the home page. Containers are your working copy of an image. They will change as you interact with them. This tab also shows the containers that you are running and allows you to stop and restart them and see message logs from running containers.