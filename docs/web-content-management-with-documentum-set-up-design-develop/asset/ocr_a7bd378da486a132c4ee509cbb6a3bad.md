templates for creating the numerous categories of web pages on the website without having to understand any web languages or scripting methodologies such as HTML and JavaScript. The Documentum templates transform the entered data into HTML web pages with the desired look and feel via presentation files (stylesheets).

You may want to refer to Chapter 10 for a quick recap of what Templates are and how they couple with other Web Publisher files in Documentum.

For your convenience, let us discuss the Web Publisher **template-rules-presentation** architecture (refer to figure 13.1) once again to fix up any missing bolts and loose ends.

![](bbox=[0.19, 0.28, 0.8, 0.42])

Figure 13.1: Web Publisher template-rules-presentation architecture

Web Publisher Template files can be created in Documentum as XML files with elements corresponding to the various fields in the template.

A **Rules file** is created in Documentum as an XML file with semantics added for the template field elements. Field type (checkbox, text box, etc.), field length, and field validations (mandatory, read-only, etc.), are defined in a Rules file.

Web Publisher internally interprets Template and Rules XML files to create and show the template fields in Web Publisher in a form that users can use to create web pages. The actual values filled in the Web Publisher editor template fields create a content file that is nothing but data assigned to the template XML elements.

**Presentation files** (XSL stylesheets) style the content XML files using Documentum's Xerces XSL Transformation engine to create web pages in the desired format/rendition (such as HTML, WML, etc.)

If you were to talk about division of responsibilities between the business users (content creators) and the IT development team, then it can be summed up as follows:

The IT development team creates object types, lifecycles, Templates, Rules files, presentation files, and other Documentum configuration items/related objects, while the business users simply use these objects and templates to fill in and create content for their website without the need to understand any programming languages/scripting/configuration settings!