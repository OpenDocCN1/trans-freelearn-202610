# Font properties

Pentaho Reporting uses Java’s built-in font support for most of its font operations. Some additional functionality exists in `libfonts`, providing the report engine with additional information that is not available through Java’s standard `Font` and `FontMetrics` API.

In Pentaho Report Designer, fonts may be modified in multiple ways. First, all text-based elements contain font properties. These properties allow the selection of font family, font size, and font style. When editing these properties through the menu Format | Font..., the following screen is presented:

![](bbox=[0.12, 0.38, 0.65, 0.66])

In addition to modifying the property, Pentaho Report Designer also provides a toolbar (shown in the following screenshot), where you can quickly modify the font name, size, style, and alignment:

![](bbox=[0.48, 0.72, 0.87, 0.75])

Another way to configure fonts is by using the element property that has the following properties available:

| Name | Type | Description |
|------|------|-------------|
|      |      |             |