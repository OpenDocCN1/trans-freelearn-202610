![](bbox=[0.13, 0.06, 0.86, 0.4])

As you can see, these platforms have different color schemes. By default, the Windows Phone color scheme is like Android in that it displays light text on a dark background, but this color scheme is changeable by the user. Even on a Windows Phone emulator, you can change the color scheme in the Themes section of the Settings application, and then rerun the program.

The app is not only run on the device or emulator, but deployed. It shows up along with the other apps on the phone or emulator and can be run from there. If you don’t like the application icon or how the app name shows up, you can change that in the individual platform projects.

# Anatomy of a Xamarin.Forms solution

Obviously, the program created by the Xamarin.Forms template is very simple, so this is an excellent opportunity to examine the generated code files and figure out their interrelationships and how they work.

Let’s begin with the code that’s responsible for defining the text that you see on the screen. This is the App.cs file in the Hello project, and if the project template hasn’t changed too much since this chapter was written, it probably looks something like this:

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

using Xamarin.Forms;

namespace Hello
```