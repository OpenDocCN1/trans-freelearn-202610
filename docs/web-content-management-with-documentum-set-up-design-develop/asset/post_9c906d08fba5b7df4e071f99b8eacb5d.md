
Let us now discuss figure 24.3 in detail:

- **Components**: These consist of JSP pages and the controls within these JSP pages. Components are reusable units in the WDK framework and perform Docbase operations such as check-out, check-in, export, etc. Each WDK component consists of an XML configuration file, an associated behavior Java class, and at least one JSP page.
- **Controls**: These represent UI features (buttons or links) and provide standard web functionality and operations such as validations, formatting, etc. Controls can be reused in multiple JSP pages and raise events due to changes made to these elements in UI forms.
- **Forms (JSP pages)**: WDK models JSP pages as Form objects on the server to serve two purposes:
  - Maintaining the state of the HTML form
  - Maintaining the browser history
- **Actions**: Actions are launched either by a Docbase operation (such as check-out) or via a UI form element (such as button, menu item, etc.). Actions relate a UI event with an application function.
- **Events**: Events are raised whenever a user action causes changes in the element states in a UI form. Events are either handled client-side via JavaScript in the associated JSP page or on the server, by the associated component.

Some of these terms might not be very clear right now. However, you may go through the Documentum WDK Development guide to get a better understanding. As we proceed with the examples in this chapter, these concepts will definitely become much clearer.

## 24.3 A Simple WDK Configuration Example

Without spending any more time, let us start off with a very simple example of WDK configuration.

We will override the default New Content (properties) screen in Web Publisher and introduce new attributes for the News Article template.

### 24.3.1 New Content Screen before Configuration Changes

If you create new content in Web Publisher using a template category, the New Content (properties) screen is shown to you for providing properties (metadata) information for the content such as:

- Name of content (attribute: `object_name`)
- Descriptive name (attribute: `title`)
- Subject (attribute: `subject`)
- Effective date (attribute: `a_effective_date`)
- Expiration date (attribute: `a_expiration_date`)

Figure 24.4 depicts how this default screen appears in Web Publisher.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_389_0.png)

Figure 24.4: Default New Content screen in Web Publisher

### 24.3.2 Modified New Content Screen after Configuration Changes

Figure 24.5 shows the same New Content screen for the News Article template with the newly added attributes.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_390_0.png)

Figure 24.5: Modified New Content screen with newly added attributes for News Article

The following steps will explain how to achieve this simple configuration in Documentum using the WDK framework.

1. Copy the XML configuration file for the New Content component (newcontent_component.xml) from the following location in your Web Publisher server:
`\wp\config\library\newcontent.`
Note that in all the examples in this chapter, we will assume that the name of the Web Publisher application is wp.
Create the following directories inside the config folder within the custom folder in Web Publisher server:
`\library\newcontent.`
Place the copied newcontent_component.xml at the following location:
`\wp\custom\config\library\newcontent.`
We will look at the changes to be made in the copied newcontent_component.xml file later in the steps below.

2. Copy the New Content JSP file (newContent.jsp) from the following location in your Web Publisher server: `\wp\library\newcontent.`
Create the following directories inside the custom folder in Web Publisher server:
`\library\newcontent.`
Rename the copied newContent.jsp as (say) custom_newContent.jsp and place it at the following location:
`\wp\custom\library\newcontent.`
We will look at the changes to be made in the custom_newContent.jsp file later in the steps below.

3. Modify the newcontent_component.xml file under `\wp\custom\config\library\newcontent` as shown below:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!------------------------------------------------------------------------------>
<config version='1.0'>
    <scope wpcontext="wpview">
        <!-- the document list component definition -->
        <!-- <component id="newcontent"> -->
        <component id="newcontent"
        extends="newcontent:wp/config/library/newcontent/
                newcontent_component.xml">
            <!-- Component Contract -->
            <params>
                <param name="objectId" required="false"/>
```

```
</params>
<!-- Component Layout -->
<pages>
<!--<start>/wp/library/newcontent/newContent.jsp</start>-->
<start>/custom/library/newcontent/custom_newContent.jsp</start>
<return>/wp/library/componentReturn.jsp</return>
</pages>

<!-- Component Behavior -->
<class>com.documentum.wp.library.newcontent.NewContent</class>
<nlsbundle>com.documentum.wp.library.newcontent.NewContentNlsProp</nlsbundle>

<!-- Component specific Configuration -->
<!-- Allow the user to import a file from file system. Default is true. -->
<allow_import>true</allow_import>

<enable_full_text_index>true</enable_full_text_index>

<!-- Component specific Configuration -->
<helpcontextid>newcontent</helpcontextid>
</component>
</scope>
</config>
```

The anatomy of this XML file is given below:

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
```

This is the standard XML declaration

```
<config version='1.0'>
```

This is the root element of the XML configuration file.

```
<scope wpcontext="wpview">
```

This is the scope of the configuration file defines the context within which this configuration definition holds good.

In the tag <scope wpcontext="wpview">, wpcontext is a qualifier name and wpview is a qualifier value. Context matching happens for qualifier values. Qualifiers are specified in the app.xml file for the respective WDK application layers.

Following are some of the qualifiers provided by the various WDK application layers such as WDK, Webtop, Web Publisher, etc:

- Docbase name
- Docbase type
- Privilege
- Application
- Web Publisher context
- Web Publisher role

You can even introduce your custom qualifiers in the WDK application if need be. Documentum resolves the scope as per the order in which the qualifiers have been specified in the app.xml file for the applications.

```
<component id="newcontent"
```

This is the primary element in the XML configuration file. Apart from component, the other valid elements can be action (Action components) and application (app.xml file). The id attribute tags the component/action name with a unique identifier in the application. You can extend other components/actions/applications by using the extends attribute in the primary element.

```
<params>
    <param name="objectId" required="false"/>
```

These lines define the parameters that are passed from the configuration to the associated behavior class.

```
<pages>
```

<pages> defines the associated JSP page(s) for the layout of component in question. You can see that we have commented out the path for default newContent.jsp and instead provided the path for our custom JSP file custom_newContent.jsp.

```
<class>com.documentum.wp.library.newcontent.NewContent</class>
```

This line defines the associated behavior Java class for the component. In this particular configuration example, since we are just modifying the xml configuration file and the JSP file for the New Content component, we are not extending the default NewContent.class.

```
<nlsbundle>com.documentum.wp.library.newcontent.NewContentNlsProp</nlsbundle>
```

This line defines the associated NLS property file for localizing the strings for the component. Note that in WDK framework, a component extending from another component inherits the strings defined in the NLS property file for the base component.

Other XML elements shown in the code are specific to the newcontent component and do not apply generically to all XML configuration files.

4. Modify the custom_newContent.jsp file under \wp\custom\library\newcontent as shown in the following code snippet.

Note that in the JSP file, we have added lines 137-151 for the following custom attributes of News Article object type (cust_newsarticle): cust_news_summary, cust_news_is_press_rel, cust_news_edit_approvers, and cust_news_display_order.

These are Docbase attributes and their behavior is handled by default by the WDK framework. We have introduced these new attributes in the JSP similar to the default implementation for attributes such as keywords and authors shown below (refer to the highlighted line).

```
<dmfx:docbaseattribute object="<%=NewContent.CONTENT_OBJECT%>"
attribute="keywords"
pre="<tr><td align='right' valign='top'><b>"
col1=":</b></td><td>&nbsp;</td><td>"/>
<tr><td colspan='4' height='5'></td></tr>
```
