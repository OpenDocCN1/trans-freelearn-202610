![](bbox=[0.293, 0.092, 0.706, 0.483])

Figure 1.19: Choosing the Main executable to run the compiled application

As the last step of the preparations, we look at the organization of the code in the GitHub repository of the book.

# Code organization in this book

The code for every chapter is stored in the GitHub repository, in a separate folder with the relevant chapter number. The number uses two digits to get the ordering right. Inside each folder, one or more subfolders can be found. These subfolders contain the code of the chapter, depending on the progress of that specific chapter.

For all chapters, we put the Main.cpp class and the CMake configuration file, CMakeLists.txt, into the project root folder. Inside the cmake folder, helper scripts for CMake are stored. These files are required to find additional header and library files.

All C++ classes are located inside folders, collecting the classes of the objects we create. The Window class will be stored in the window subfolder to hold all files related to the class itself, and the same applies to tools – the logger, the model classes, and the renderer-related classes. After you have all the required code and tools installed, let’s get a general idea of what game character animations are about.