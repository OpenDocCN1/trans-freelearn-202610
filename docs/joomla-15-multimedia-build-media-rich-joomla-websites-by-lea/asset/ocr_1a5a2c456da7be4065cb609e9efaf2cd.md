Compression works to remove the data that is considered "not important" and hence our file sizes drop due to this additional information that no longer exists. A good example of video compression is to remove the unrecognized colors from a video clip. There may be millions of colors in a movie (subtle shades of similar colors), but with humans only being able to perceive just over 1000 of them, it is in our interest to compress and remove this unwanted data.

Throw away too much data (over compression) and the changes become noticeable. When compressing video, the goal is to compress as much as possible to the point where you can notice a quality loss, and then nudge back a bit from this point, so you try to achieve optimum data compression and file size with minimal quality loss.

# Video codec

Once upon a time, video codecs were a thing for "AV Nerds" and "Hardcore Illegal Video Downloaders", but believe it or not, you probably use them often if you use or watch movies either in a cinema, on a TV, or on your mobile phone.

A **video codec** is a device or software that enables video compression, and again the decompression of this data. The term **codec** stands for compression / decompression and various codecs are used in both the audio and video media formats.

Due to large media file sizes, mathematical codecs were built to encode a signal for transmission, and then decode it again for viewing. There are hundreds of codecs being used now and many of these come included with modern computer equipment. Some of the more common video codecs are VC-1, MPEG-4, and H.264.

It is important to note that a video codec is not a video container (or wrapper) file, by this I mean the final file format. You have probably noticed that none of your video files have an extension like `.vc-1`, or `.h.264`. This is because video files are packaged in containers or wrappers that hold all sorts of file information, such as additional audio and navigational information, as well as the video file itself. The codec does the compression and the container file holds the information for that file. Examples of video container files are FLV (Flash Video), MOV (Apple QuickTime), and so on. Each of these containers uses a certain type of video codec for their compression. Further information about video codecs and container files can be found at `http://en.wikipedia.org/wiki/Category:Video_codecs`.

We will be looking at these container files very soon because they are an important format for saving our web videos.