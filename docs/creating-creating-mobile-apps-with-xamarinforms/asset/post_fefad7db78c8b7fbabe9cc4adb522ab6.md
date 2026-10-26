
![](img/c2d61d0459c51981af78607f1eef3d1c_95_0.png)

The **Button** at the bottom labeled **Save** currently does nothing. Such a button is undoubtedly intended to save the note to a file. To do that, however, we need to know how to save files, and later load them back in.

File input/output is not something included in the Xamarin.Forms API. Yet, some file I/O is required by many mobile apps, if only to save settings and interim data. For this reason, file I/O is probably the most compelling reason to skirt around the Xamarin.Forms API and implement some vital features of your app by using the programming interfaces of the individual platforms.

## Version 2. File input/output

Traditionally, file input/output is one of the most basic programming tasks, but file I/O on mobile devices is a little different than on the desktop. On the desktop, users and applications generally have a whole disk available organized in a directory structure. On mobile devices, several standard folders exist—for pictures or music, for example—but application-specific data is generally restricted to private storage areas.

Programmers familiar with .NET know that the `System.IO` namespace contains the bulk of standard file I/O support. Perhaps the most important class in this namespace is the static `File` class, which not only provides a bunch of methods to create new files and open existing files but also includes several methods capable of performing an entire file read or write operation in a single method call.

For example, the `File.WriteAllText` method has two arguments of type `string`—a filename and the file contents. The method creates the file (replacing an existing file with the same name if necessary), writes the contents to the file, and closes it. The `File.ReadAllText` method is similar but returns the contents of the file in one big `string` object. These methods seem ideal for the job of saving and retrieving notes.

The Xamarin.iOS and Xamarin.Android libraries include a version of .NET that has been expressly tailored by Xamarin for these two mobile platforms. The methods in the `File` class in the `System.IO` namespace map to appropriate file I/O functions in the iOS and Android platforms. This means that you can use methods in the `File` class—including `File.WriteAllText` and `File.ReadAllText`—in your iPhone and Android applications.

Let’s experiment a bit:

Go into Visual Studio or Xamarin Studio, and load any Xamarin.Forms solution created so far, such as **NoteTaker1**. Bring up one of the code files in the iOS or Android project. In a constructor or method, type the `System.IO` namespace name and then a period. You’ll get a list of all the available types in the namespace. If you then type `File` and a period, you’ll get all the static methods in the `File` class, including `WriteAllText` and `ReadAllText`.

In a Windows Phone project, however, you’re working with a version of .NET created by Microsoft and stripped down somewhat for the Windows Phone platform. If you type `System.IO.File` and a period, you’ll see a rather diminished `File` class that does *not* include `WriteAllText` and `ReadAllText`, although it does include methods to create and open text files.

Now go into any code file in a Xamarin.Forms Portable Class Library project, and type `System.IO` and a period. Now you won’t even see the `File` class! It does not exist in the PCL. Why is that? PCLs are configured to support multiple target platforms. The APIs implemented within the PCL are necessarily an intersection of the APIs in these target platforms.

A PCL appropriate for Xamarin.Forms includes the following platforms:

- .NET Framework 4.5
- Windows 8
- Windows Phone Silverlight 8
- Xamarin.Android
- Xamarin.iOS

Notice the inclusion of Windows 8, which incorporates an API called the Windows Runtime (or WinRT). Microsoft completely revamped file I/O for WinRT and created a whole new file I/O API. The `System.IO.File` class does not exist in the PCL because it is not part of WinRT.

Although the `File` class does not exist in a Portable Class Library project, you might wonder what kind of `File` class you can use in a Shared Asset Project. Well, it varies by what platform is being compiled. You can use `File.WriteAllText` and `File.ReadAllText` in your iOS and Android projects but not in your Windows Phone projects. Your Windows Phone projects need something else.

### Skip past the scary stuff?

Already you might suspect that this subject of file I/O is going to get hairy, and you are correct. But consider what we’re trying to do here: We’re trying to target three different mobile platforms with a common code base. That’s not easy, and we’re bound to encounter some rough terrain along the way.

So the question has to be: Metaphorically speaking, do you enjoy hiking through treacherous terrain to climb to the top of a mountain and get a gorgeous view? Or would you prefer that somebody else takes a photo from the top of the mountain and sends it to you in an email?

If you’re in the latter category, you might prefer to skip over much of this chapter and jump straight to the comparatively simple and elegant solution to the problem of Xamarin.Forms file I/O presented at the chapter’s end and continuing into the next chapter. Until that point, some of the transitional code you’ll encounter will be both scary and ugly.

If you choose to take this long path up the mountain, you’ll understand why the trip is necessary, and you’ll understand the rationale behind the platform differences—even in seemingly routine jobs like file I/O.

In the previous chapter you saw how the Xamarin.Forms Device class can be a valuable tool for dealing with platform differences. But the code that’s referenced by the Device class must be compilable in all three platforms. This is not the case for file I/O because the different platforms have access to different APIs. This means that the platform differences can’t be managed using the Device class and must be handled in other ways. Moreover, the solutions are different for Shared Asset Projects and Portable Class Libraries projects.

For this reason, for the next two versions of NoteTaker, there will be separate solutions for SAP and PCL. The two different solutions for version 2 are named NoteTaker2Sap and NoteTaker2Pcl.

### Preprocessing in the SAP

Dealing with platform differences in a Shared Asset Project is a little more straightforward than a PCL and involves more traditional programming tools, so let’s begin with that.

In code files in a Shared Asset Project, you can use the C# preprocessor directives `#if`, `#elif`, `#else`, and `#endif` with conditional compilation symbols defined for the three platforms. These symbols are `__IOS__` for iOS and `WINDOWS_PHONE` for Windows Phone; there are no conditional compilation symbols for Android, but Android can be identified as not being iOS or Windows Phone.

The NoteTaker2Sap project includes a class named `FileHelper` in a file named `FileHelper.cs`. You can add such a file to the project the same way you add a new file for the class that derives from `ContentPage`.

The `FileHelper.cs` file uses C# preprocessor directives to divide the code into two sections. The first section is for iOS and Android and is compiled if the `WINDOWS_PHONE` identifier is not defined. The second section is for Windows Phone.

Both sections contain three public static methods named `Exists`, `WriteAllText`, and `ReadAllText`. In the first section, the iOS and Android versions of these functions use standard static `File` methods but with a folder name obtained from the `Environment.GetFolderPath` method with an argument of `Environment.SpecialFolder.MyDocuments`:

```
namespace NoteTaker2Sap
{
    static class FileHelper
    {
#if !WINDOWS_PHONE // iOS and Android
        public static bool Exists(string filename)
        {
            string filepath = GetFilePath(filename);
            return File.Exists(filepath);
        }

        public static void WriteAllText(string filename, string text)
        {
            string filepath = GetFilePath(filename);
            File.WriteAllText(filepath, text);
        }

        public static string ReadAllText(string filename)
        {
            string filepath = GetFilePath(filename);
            return File.ReadAllText(filepath);
        }

        static string GetFilePath(string filename)
        {
            string docsPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
            return Path.Combine(docsPath, filename);
        }

#else // Windows Phone
        public static bool Exists(string filename)
        {
            return File.Exists(filename);
        }

        public static void WriteAllText(string filename, string text)
        {
            StreamWriter writer = File.CreateText(filename);
            writer.Write(text);
            writer.Close();
        }

        public static string ReadAllText(string filename)
        {
            StreamReader reader = File.OpenText(filename);
            string text = reader.ReadToEnd();
        }
    }
}
```
