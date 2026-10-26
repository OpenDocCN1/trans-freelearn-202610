# The interactive interface

Many Xamarin.Forms views are interactive and respond to touch gestures—tapping and dragging—and a few read input from the keyboard.

These interactive views incorporate paradigms and even names that are familiar to both users and programmers: Users can trigger commands with `Button`, specify a number from a range of values with `Slider` and `Stepper`, enter text from the phone’s keyboard using `Entry` and `Editor`, and select items from a collection with `Picker`, `ListView`, and `TableView`.

This chapter is devoted to demonstrating the use of several of these interactive views. You’ve already seen the `Entry` and `Editor`, and detailed coverage of the various collection views won’t appear until a chapter not in the Preview Edition, but this chapter explores several popular interactive views. In the process, the chapter goes deeper into data binding and introduces the powerful `Grid` layout.

# View overview

The initial releases of Xamarin.Forms includes 19 instantiable classes that derive from `View` but not from `Layout`. You’ve already seen seven of these classes in the previous chapters: `Label`, `BoxView`, `Button`, `Entry`, `Editor`, `ListView`, and `Image`.

Seven views (including two you’ve already seen) allow the user to select or interact with basic .NET and C# data types:

| Data Type | Views |
|---|---|
| `Double` / `double` | `Slider`, `Stepper` |
| `String` / `string` | `Entry`, `Editor` |
| `Boolean` / `bool` | `Switch` |
| `DateTime` | `DatePicker`, `TimePicker` |

Much of the focus of this chapter is devoted to these views. The remaining views not discussed yet are:

+   - `SearchBar`, a single-line text entry with a button specifically for the purpose of initiating a search.
+   - `WebView`, to display web pages or HTML.