Storing the same data in a binary format would also be possible, for instance, when decoding the text information will take too long.

But creating an entirely new file format should the last resort since you will have to write all the code to read and write the data, keep track of different versions of the file format during read and write operations, and maybe even support different operating systems and hardware architectures. Maintaining such an organically grown file format can become a nightmare.

A better way is to use standard formats and bundle all files into an archive, for instance, into a ZIP file. You don’t have to care about missing files when the level is distributed to players or other developers, but at the same time, you do not need to reinvent the wheel by creating a new, all-encompassing file format.

Such archives are more common than you might think. Formats such as WAD from the original DOOM and PAK/PK2/PK3 for the Quake series were created to collect all game data in a single file, and these file types even support patching as files in new archives replace files of the same name in old archives.

Building your own levels

What if you can’t find a suitable game level map on the internet? You still have the option to create a small map by yourself, for instance, with Blender. Creating a map is out of the scope of this book, but you will find suitable tutorials and videos on the internet. You can find links to two example videos in the Additional resources section.

After we explored the reasons why separating model and level data in the application, we will now implement new code for loading and processing level data.

# Importing a game map

As the first step to load a level, we will add a new C++ class named AssimpLevel. You can think of the AssimpLevel class as a mix of the two classes, AssimpModel and AssimpInstance, containing the static vertex data of the model class and dynamic properties such as the position, rotation, or scale of the instance class.

The AssimpLevel class consists of two new files, AssimpLevel.h and AssimpLevel.cpp. Both files are in the model folder, but we will borrow most of the methods and members from the two classes, AssimpModel and AssimpInstance.

Let’s do a brief walkthrough of the AssimpLevel class.