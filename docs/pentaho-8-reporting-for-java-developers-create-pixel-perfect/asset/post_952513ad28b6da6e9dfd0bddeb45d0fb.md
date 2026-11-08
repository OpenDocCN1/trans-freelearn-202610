
| Name | Description |
|------|-------------|
| `report-header` | The `report-header` element specifies the layout of the report header. This element contains a child element titled `root-level-content`, which contains all references to child elements. Note that `report-header` is a report element. Therefore, it may contain general element attributes and child elements, defined later in the *Report elements* section of this chapter. |
| `group` | The `group` element defines the entire hierarchy of groups and detail bands defined in a report. This element hierarchy is defined in more detail in the *Group and detail band hierarchy* section of this chapter. |
| `report-footer` | The `report-footer` element specifies the layout of the report footer. This element contains a child element titled `root-level-content`, which contains all references to child elements. Note that `report-footer` is a report element. Therefore, it may contain general element attributes and child elements, defined later in the *Report elements* section of this chapter. |

Also note that the report itself is represented as a report element. Therefore, in addition to the attributes and child elements defined previously, the report may also contain attributes and elements defined in report elements (these are covered later in the *Report elements* section of this chapter).

## Group and detail band hierarchy

A group may contain either a `group-body` or `data-body` child XML element. A `group-body` contains the XML element `group`, used to represent a hierarchy of groupings. In addition to containing `group-body` or `data-body`, a `group` XML element may also contain the `group-header` and `group-footer` XML elements. The `group-header` and `group-footer` XML elements may contain a `root-level-content` XML element, which contains child layout elements.

The `data-body` XML element contains the bands relevant to row level data in your report. This XML element may contain `detail-header`, `details`, `no-data`, and `details-footer` child XML elements. Each of these elements may contain a `root-level-content` XML element, which contains child layout elements.

## Report elements

Each report element defined in the previous chapters is represented in XML, defining its properties, as well as where it is located within a band. The most common elements are highlighted here.

All elements define the style:element-style child XML element, which contains styling information for the individual element. The most common style element is style:spatial-styles, which defines where in the band the element is located, as well as what size it should render as:

```
<style:element-style>
  <style:spatial-styles x="20" y="240" min-width="100" min-height="20"/>
</style:element-style>
```

The following is a list of some of the primary report elements, along with specific details on creating them:

| Name | Description |
|------|-------------|
| label | The `label` XML element renders a label. This element contains a `core:value` child element, which uses XML text for the label value. |
| text-field | The `text-field` XML element renders a text field. This element contains a `core:field` attribute, which defines the field to render. |
| number-field | The `number-field` XML element renders a number field. This element contains a `core:field` attribute, which defines the field to render, as well as a `core:format-string` attribute, which defines the number format string to render. |
| date-field | The `date-field` XML element renders a date field. This element contains a `core:field` attribute, which defines the field to render, as well as a `core:format-string` attribute, which defines the date format string to render. |
| message | The `message` XML element renders a message. This element contains a `core:value` child element. |
| resource-label | The `resource-label` XML element renders a resource label. This element contains a `core:resource-identifier` attribute referencing the message bundle, as well as a `core:value` attribute, which specifies the key to use in the bundle. |
| resource-field | The `resource-field` XML element renders a resource field. This element contains a `core:resource-identifier` attribute referencing the message bundle, as well as a `core:field` attribute, which specifies the field that is used to determine the key to be resolved. |
| resource-message | The `resource-message` XML element renders a resource message. This element contains a `core:resource-identifier` attribute referencing the message bundle, as well as a `core:value` attribute, which specifies the message. |
| rectangle | The `rectangle` XML element renders a rectangle. The size of the rectangle is determined by the `style:spatial-sizes` element defined earlier. A `style:content-styles` XML element may be defined, which specifies attributes such as the Boolean attributes `draw-shape` and `fill-shape`, as well as color attributes such as `fill-color`. Colors are represented in RGB hexadecimal notation, for instance `#ff3333`. |
| horizontal-line | The `horizontal-line` XML element renders a horizontal line. The length of the line is determined by the `style:spatial-sizes` XML element defined earlier. A `style:content-styles` XML element is defined to specify the line style and color. |
| vertical-line | The `vertical-line` XML element renders a vertical line. The length of the line is determined by the `style:spatial-sizes` XML element defined earlier. A `style:content-styles` XML element is defined to specify the line style and color. |
| sub-report | A `sub-report` XML element renders a subreport. The attribute `href` specifies where the subreport is located in the `.prpt` bundle. This XML element may also contain information about its location and style via the `style:element-style` child XML element, as well as input and output parameter mappings via the `input-parameter` and `output-parameter` XML elements. These mappings are configured with the `master-fieldname` and `detail-fieldname` attributes. |
| content | A `content` XML element renders an image. The location of the image binary is determined by the `core:value` XML element. The scale and aspect ratio properties for a `content` element may be customized by specifying a `style:content-styles` element, with the attributes `scale` and `keep-aspect-ratio`. |
| content-field | A `content-field` XML element renders a dynamic image-based on a field. The location of the dynamic content to render is determined by the `core:field` XML element. The scale and aspect ratio properties for a `content-field` element may be customized by specifying a `style:content-styles` element, with the attributes `scale` and `keep-aspect-ratio`. |

Additional report element types are defined in Pentaho Reporting. Those listed in the table are the most common elements, along with some of their primary properties and styles.

## settings.xml file

The `settings.xml` file contains global configuration properties for a report, as well as a placeholder for future runtime information.

One example of a configuration setting is related to the execution of formula expressions. If you set the property `org.pentaho.reporting.engine.classic.core.function.LogFormulaFailureCause` to true, and you have your Log4j logging set to debug, you will receive a stack trace, in addition to an error message, when the formula fails.

Additional configuration properties are available, primarily for use in design tools such as Pentaho Report Designer. Rarely would you need to define a configuration property while generating a report.

## styles.xml file

The `styles.xml` file defines what page format the report should be rendered as, style rules that dictate how a report should be rendered, global layout processors, a report watermark, and finally the page header and footer sections of a report.

The `styles.xml` file contains the `style` root XML element, and includes namespaces relevant to rendering styles, including the `core` and `layout` namespaces defined earlier, as well as the `style` namespace as the default namespace, which is located at http://reporting.pentaho.org/namespaces/engine/classic/bundle/style/1.0. The following child elements may be specified in the `style` XML element:

| Name | Description |
|------|-------------|
| `page-definition` | The `page-definition` XML element contains attributes defining the report's page properties. The attribute `pageformat` specifies the page format type. The attributes `horizontal-span` and `vertical-span` define how many pages a report should span across. The `width` and `height` attributes define the width and height of the paper. The `orientation` attribute defines whether the page should be laid out as portrait, landscape, or reverse-landscape. The attributes `margin-top`, `margin-left`, `margin-bottom`, and `margin-right` define the margin values for the report. |
| `style-rule` | Multiple `style-rules` may be defined in the report. A style rule has the `name` and `parent` attributes, which define the inheritance tree of a group of styles. Within the `style-rule`, style elements such as `text-styles` and `spatial-styles` are defined, which contain specific styling information. |
| `layout:layout-processors` | Certain functions that manipulate the layout of the entire report may appear in the `layout-processors` section of the styles document. |
| `watermark` | The `watermark` XML element is rendered in the background, behind |
