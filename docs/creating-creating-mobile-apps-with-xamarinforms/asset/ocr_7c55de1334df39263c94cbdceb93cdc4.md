problems that we battle through our ignorance to acquire knowledge and skills.

In this chapter the **NoteTaker** program goes through six different versions. They are numbered 1 through 6, but the version numbers should probably be more like 0.1 through 0.6. In the course of enhancing this program through these six versions, the following topics are encountered:

+   - The `Entry` and `Editor` views for editing text
+   - File input/output (I/O) in the mobile environment
+   - Asynchronous operations
+   - Property change notifications
+   - Data binding

The next chapter gets into multi-page architectures, page navigation, the powerful `ListView` for displaying collections of items, and dealing with application lifecycle issues.

# Version 1. The Entry and Editor views

The **NoteTaker** app definitely requires some text input. Some phones have physical keyboards, but the vast majority are limited to virtual onscreen keyboards. Of course, these are somewhat different for each platform and often also vary by the type of text input.

Xamarin.Forms defines two views for obtaining text input:

+   - `Entry` for a single line of text
+   - `Editor` for multiple lines of text

Both `Entry` and `Editor` derive from `InputView`, which derives from `View`.

All three platforms have various styles of virtual keyboard appropriate for different types of text input. For example, a keyboard for typing a URL should be different from a keyboard for entering a phone number. For this reason, `InputView` defines a property named `Keyboard` of type `Keyboard`, a class that defines seven static read-only properties of type `Keyboard` appropriate for different keyboard uses:

+   - `Default`
+   - `Text`
+   - `Chat`
+   - `Url`
+   - `Email`