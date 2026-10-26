The next code snippet (to be executed in a separate Google Colab code cell) sets the working directory to the previously created folder, so the downloaded results remain in this folder when the Google Colab instance stops:

```
cd "/gdrive/My Drive/Detectron2/Chapter3"
```

It is always a good practice to ensure that Google Colab is currently working in this directory by running the pwd() command in another code cell and inspecting the output.

Once you are in the specified folder, the following code snippet helps to download images from Google Images using the simple_image_download package:

```
from tqdm import tqdm
from simple_image_download.simple_image_download import simple_image_download as Downloader
def _download(keyword, limit):
    downloader = Downloader()
    downloader.download(keywords=keyword, limit=limit)
    urls = downloader.urls(keywords=keyword, limit=limit)
    return urls

def download(keywords, limit):
    for keyword in tqdm(keywords):
        urls = _download(keyword=keyword, limit=limit)
        with open(f"simple_images/{keyword}.txt", "w") as f:
            f.writelines('\n'.join(urls))
```

Specifically, the first import statement helps to import the tqdm package, which can be used to show downloads' progress. This is important because downloading a large number of images from the internet would take time. Therefore, it would be helpful to have a progress bar showing us that it is currently working and approximately how long it will take to complete the task. The next import statement imports the simple_image_download object, which helps to perform the downloading task. The next is the _download helper method to download a number (specified by the limit parameter) of images from Google using a keyword (specified by the keyword parameter). Furthermore, for the corresponding pictures, this helper method also returns an array of URLs that can be stored to validate the downloaded images and check for copyrights if needed.