resembles a horizontal rule.

The resultant font sizes are perhaps not as consistent between the three platforms as you might prefer, but they provide a rough idea what you can expect:

![](bbox=[0.129, 0.147, 0.856, 0.491])

Interestingly, the numeric sizes calculated using the device resolution seem a bit more consistent among the platforms than the named sizes, at least on these devices.

You might need to fit a block of text to a particular rectangular area. Two numbers can help with this job:

The first is line spacing. This is the vertical height of a `Label` view per line of text. It is roughly related to the font size specified in the `Font.SystemFontOfSize` method as follows:

+   - iOS: *LineSpacing* = 1.2 * *FontSize*
+   - Android: *LineSpacing* = 1.2 * *FontSize*
+   - Windows Phone: *LineSpacing* = 1.3 * *FontSize*

The second helpful number is average character width. This is about half of the font size, regardless of the platform:

+   - *AverageCharacterWidth* = 0.5 * *FontSize*

For example, suppose you want to fit a text string containing 80 characters in a width of 320 units. You’d like the font size to be as large as possible. Divide the width (320) by half the number of characters (40), and you get a font size of 8 that you can use in the `Font.SystemFontOfSize` method. For text that’s somewhat indeterminate and can’t be tested beforehand, you might want to