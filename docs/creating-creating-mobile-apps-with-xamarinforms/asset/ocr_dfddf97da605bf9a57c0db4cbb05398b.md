![](bbox=[0.13, 0.06, 0.86, 0.4])

The most disappointing version of this Greetings program is definitely the iPhone: In iOS 7, a single-page application shares the screen with the status bar at the top. Anything the application displays at the top of its page will occupy the same space as the status bar unless the application compensates for it.

This problem disappears in multipage navigation applications, but until that time, here are four ways to solve this problem right away:

# 1. Include padding on the page

The Page class defines a property named Padding that marks an area around the interior perimeter of the page into which content cannot intrude. The Padding property is of type Thickness, a structure that defines four properties named Left, Top, Right, Bottom. (You might want to memorize that order because that’s the order you’ll define them in the Thickness constructor as well as in XAML.) The Thickness structure also defines constructors for setting the same amount of padding on all four sides or for setting the same amount on the left and right and on the top and bottom.

A little research in your favorite search engine will reveal that the iOS 7 status bar has a height of 20. (Twenty what? you might ask. Twenty pixels? Actually no. For now, just think of them as 20 “units.” For most Xamarin.Forms programming you shouldn’t need to bother with numeric sizes, but Chapter 5 will provide some guidance when you need to get down to the pixel level.) You can accommodate the status bar like so:

```
class GreetingsPage : ContentPage
{
    public GreetingsPage()
```