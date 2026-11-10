
## Automatic Property Extraction (APE)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE application SYSTEM "config_5.2.dtd">
<application>
    <name>TwoWayXMLApp</name>
    <app_pattern>
        <element>INFORMATION</element>      <!-- Specifying the XML Root element INFORMATION -->
    </app_pattern>
    <map_rules>
        <xml_content_rule>
            <element_selection_pattern>
                <element>INFORMATION</element>
            </element_selection_pattern>
            <variables>      <!-- Defining variable to map values in object attribute subject -->
                <variable>
                    <name>subjectValue</name>      <!--Getting the value of SUBJECT template element -->
                    <content_of_element>
                        <element_selection_pattern>
                            <element>SUBJECT</element>
                        </element_selection_pattern>
                    </content_of_element>
                </variable>
            </variables>
            <object_type>dm_document</object_type>      <!-- Specifying the object type -->
            <metadata>
                <two_way_dctmattr>
                    <name>subject</name>    <!-- Saving value in subject attribute-->
                    <var name="subjectValue"/>
                </two_way_dctmattr>
            </metadata>
            <make_entity/>
        </xml_content_rule>
    </map_rules>
</application>
```

In this code, <two_way_dctmattr> sets the value of a single Documentum object property and the mapped Template XML file element.

Here we have mapped the value of the subject property through the variable subjectvalue to the value entered by content creators in the SUBJECT element in the Template file.

> In the case of repeating valued Documentum object properties, use the <two_way_dctmattr_repeating> element in your XML configuration file.

- 4. Complete all the required steps for configuring this XML Application, exactly the same way as we did for our earlier XML Applications.

### 18.7 Testing the Two-Way Attribute Extraction XML Application:

Let us create sample content in Web Publisher using the Two_way_Template.xml template, in order to study how the two-way property extraction works in Documentum.

Create some new content as shown in figure 18.13 and click on Ok.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_302_0.png)

Figure 18.13: Creating new content

Enter some value in the Subject field shown in the template (refer to figure 18.14) and save the data.

## Automatic Property Extraction (APE)

![](img/b18244e6621ecc39cef03ff6c0e55e7c_303_0.png)

Figure 18.14: Value for Subject entered in the template

After closing the template, click on the info icon (i) against the newly created content. You can see that its Subject property is automatically populated with the value the user had filled out in the template, as shown in figure 18.15.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_303_1.png)

Figure 18.15: Subject property auto-populated in property screen

Enter a new value in the Subject property (refer to figure 18.16) and click on Ok. Open the Template file for the content again.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_304_0.png)

Figure 18.16: Modifying the value of the Subject property

You can verify that the value for the subject property that you provided in the property screen has automatically populated the Subject: field in the template as shown in figure 18.17.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_305_0.png)

Figure 18.17: The value entered via the property screen is seen in the template field.

There is much more to Automatic Property Extraction than what we have just learned in this chapter. However, to begin with, this chapter will have definitely helped you gauge the rich arena of features available in XML Applications. If need be, read more about XML Applications in the Documentum manuals to get an extensive understanding.

### 18.8 Summary

Automatic Property Extraction (APE) is a functionality that allows you to automatically assign the value of a specified template XML element to a specified document attribute and vice versa.

During our tour in this chapter, we saw that APE works only with the following Web Publisher widgets: Textline, checkbox, and choice.

We further saw how Documentum uses dm_xml_application and dm_xml_config objects to manage APE.

We then studied a simple example to create an XML application in Documentum and tested it for sample content created in Web Publisher. We also looked at examples using APE to populate repeating attributes and for two-way attribute extraction.

## 19 Working with Workflows

We have already created a custom lifecycle and associated it with our custom News Article object type in Chapters 11 and 12. This means that content created from the News Article object type (and template) can be routed through the different states of the lifecycle, such as WIP, Staging, and Approved. Have you ever thought about how content automatically progresses through the different lifecycle states? You can certainly select content items and manually promote, demote or power-promote them to move them across various linear lifecycle states. However, in an actual scenario your business users would want an automated process to achieve this. What the business teams typically want is a controlled and automated mechanism whereby content created by a group of users can be sent across electronically to a reviewer group for an initial round of review. The number of sequential/parallel groups of reviewers or approvers could be specific to a particular business. Once the content is finally reviewed and approved, it should be made available on the live website and should be in the terminal lifecycle state (say Approved/Active). At any point of time during the review and approval process, the content should be in the intermediate lifecycle states such as WIP or Staging.

This automated business process required to route content through its numerous lifecycle stages before publishing it to the production website is termed **workflow** in Documentum lingo.

A workflow goes hand in hand with a lifecycle and defines business policies for teams by having the content creators and reviewers adhere to the norms laid down by the organization.

Note that while a lifecycle simply defines the different stages in the life of a content item, a workflow actually defines how content moves through these different stages, who makes it move under various activities, what actions are performed by individual groups and users during the different activities, etc.

In Documentum, a workflow template is used to create various workflow instances (`dm_workflow`) for routing content in lifecycle stages and exists in the Docbase as a `dm_process` object. The Documentum Workflow Manager tool used to create and manage workflow templates is available once you install/configure Documentum Application Builder. (Please refer to Chapter 8 for further details.)

You could create a custom workflow as per your specific business needs or use one of the following workflows available out-of-the-box in Web Publisher:

- French Translation Workflow
- Request new content
- Submit to website
- Translate and submit to website

Launch Documentum Application Builder and open the WebPublisher DocApp under /System/Applications/webPublisher to view the default Web Publisher workflows as shown in figure 19.1.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_307_0.png)

Double-clicking on the above workflow templates launches the Workflow Manager tool and displays the workflow process definition in the form of a GUI layout.

### 19.1 Designing Custom Workflows

It is essential to design your workflows well before jumping off to creating them. Understand the need for workflows from your business (end) users. Understand how many levels of review they need and what actions need to be formed on content by which groups/users before publishing the content to a running website.

We will take a simple example in this chapter and create a workflow with the activities/tasks mentioned below. (Make sure that you customize/extend this sample workflow as per your business requirements by removing/altering existing activities or adding some more activities if need be.)

- 1. Content created by content authors will be submitted to the workflow.
- 2. The system automatically promotes the content to the Staging lifecycle state at this point.
- 3. Content (in the form of workflow tasks) reaches the Inboxes of all content manager group users for a round of review.
- 4. The first content manager user to acquire the workflow task gets to review it.
- 5. The content manager can review the content and, if it is fine, can submit it.
- 6. The system automatically promotes the content to the Approved lifecycle state, marking it finally approved and ready to be turned Active based on the specified effective date for the content.
