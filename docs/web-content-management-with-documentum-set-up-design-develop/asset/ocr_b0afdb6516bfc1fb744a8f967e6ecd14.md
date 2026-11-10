+   2. Create an XML configuration file SecondXMLApp.xml and provide the name of the XML configuration file (without the .xml) in the Category field of the template Second_XMLApp_Template.xml. This sets the a_category attribute of the template.

The SecondXMLApp.xml file is shown below:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE application SYSTEM "config_5.2.dtd">
<application>
  <name>SecondXMLApp</name>
  <app_pattern>
    <element>BOOK</element>    <!-- Specifying the XML Root element BOOK-->
  </app_pattern>

  <map_rules>
    <xml_content_rule>
      <element_selection_pattern>
        <element>BOOK</element>
      </element_selection_pattern>

      <variables>    <!-- Defining variable to map values in object attribute authors-->
        <variable>
          <name>authorName</name>    <!--Getting the value of AUTHORNAME template element -->
          <content_of_element>
            <element_selection_pattern>
              <element>AUTHORNAME</element>
            </element_selection_pattern>
          </content_of_element>
        </variable>
      </variables>
      <object_type>dm_document</object_type>    <!-- Specifying the object type -->
      <metadata>
        <dctm attr_repeating>
          <name>authors</name>    <!-- Saving value in authors repeating attribute-->
          <template><var name="authorName"/></template>
        </dctm attr_repeating>
      </metadata>
      <make_entity/>
    </xml_content_rule>
  </map_rules>
</application>
```

<dctm attr_repeating>: Sets the value of a multi-valued (repeating) Documentum object property.

Here, we have set the value of the authors multi-valued property through the variable authorName.

The variable authorName captures the multiple values entered by content creators in the AUTHORNAME repeating element in the Template file.