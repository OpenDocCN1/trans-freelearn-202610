# Version 11. Constructing a toolbar

HomePage has an **Add New Note** button, and NotePage has two buttons for **Cancel** and **Delete**. These shouldn’t really be buttons. They should be toolbar items. You can add toolbar items to a ContentPage in Windows Phone, but iOS and Android restrict toolbar items to a NavigationPage or a page navigated to from a NavigationPage.

The `ToolbarItem` class does not derive from `View` like `Button` and `ListView`. It’s really just a class for defining the characteristics of a toolbar item using the following properties:

+   - `Name` — the text that might appear
+   - `Icon` — the filename of a bitmap (most commonly PNG) stored as a resource
+   - `Order` — a member of the `ToolbarItemOrder` enumeration: `Default`, `Primary`, or `Secondary`
+   - `Priority` — an integer

The `Order` property governs whether the `ToolbarItem` appears as an image (`Primary`) or text (`Secondary`). The Windows Phone is limited to four `Primary` items, and both the iPhone and Android start getting crowded with more than that, so that’s a reasonable limitation. Additional `Secondary` items are text only. On the iPhone they appear underneath the `Primary` items; on Android and Windows Phone they aren’t seen on the screen until the user taps a vertical or horizontal ellipsis.

The `Icon` property is crucial for `Primary` items, of course, and the `Name` property is crucial for `Secondary` items, but Windows Phone also uses the `Name` to display a short text hint underneath the icons for `Primary` items.

When the `ToolbarItem` is tapped, it fires an `Activated` event rather than a `Clicked` event like the `Button` but the concept is the same. (`ToolbarItem` also has `Command` and `CommandParameter` properties like the `Button`; these are for data binding purposes and will be demonstrated in a later chapter not in this Preview Edition.)

The `ToolbarItem` constructor lets you specify all these properties (with the exception of `Command` and `CommandParameter`) along with a handler for the `Activated` event.

An instance of `ToolbarItem` cannot be added to a `StackLayout` like a view. It must be added to the `ToolbarItems` property of `Page`. This `ToolbarItems` property is of type `IList<ToolbarItem>`. Once you add a `ToolbarItem` to this collection, the `ToolbarItem` properties cannot be changed. The property settings are instead used internally to construct platform-specific objects.

Certainly the most difficult aspect of using `ToolbarItem` is assembling the bitmap images for the `Icon` property. Each platform has different requirements for the color composition and size of these icons, and each platform has somewhat different conventions for the imagery. The standard icon for