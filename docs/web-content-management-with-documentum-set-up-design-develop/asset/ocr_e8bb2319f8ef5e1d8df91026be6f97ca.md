On similar lines, <update-element-value> works fine if and only if the XML element being updated is not empty. In short, it has some text existing in the element (which might include white spaces). The following <BOOK> element cannot be updated:

```
<BOOK></BOOK>
```

However, the following XML element is a valid candidate for <update-element-value>:

```
<BOOK>ABC</BOOK>
```

For those conversant with XML/XSL, relative path does not work with Instruction Files. The XML Path Language (XPath) // (double slash) expression fails while working with Instruction Files and should not be used as a rule.

An XML file cannot be updated with an Instruction File in the following conditions:

+   - If the XML file is in the Staging state of its lifecycle
+   - If the XML file is in a checked-out state
+   - If the XML file is a part of a Change Set

An Instruction File always versions a file (i.e. provides a new version number to an XML file) even if the instructions do not make any changes to the structure of the XML file in question. Also, the files are placed in the WIP state of their lifecycle.

For example, let's take a scenario where the <delete-attribute> instruction is used in an Instruction File to remove the attribute bookname from an XML element <BOOK>.

If the <BOOK> XML element does not contain any attribute by the name bookname, the instruction <delete-attribute> still works on the <BOOK> XML element, and when it does not find any attribute by the name bookname, it still saves the XML file as a new version.

# 17.2 Instruction File Examples

Let us take a few examples to understand how Instruction Files work in Documentum.

# 17.2.1 Deleting an XML Element from an XML File with <delete-element>

Let us walk through a very simple example where content is created from a simple template XML file and later an XML element is removed from the template file. The existing content is updated via a simple Instruction File using the <delete-element> instruction.

+   1. Create a Template file (original_Template.xml) and its associated Rules file (original_TemplateRules.xml) as per the following structure:

Template File original_Template.xml:

```
<?xml version="1.0" encoding="UTF-8"?>
<PRODUCTINFO>
  <NAME/>
  <SHORTDESC/>
  <COST/>
  <PRODUCTTYPE/>
</PRODUCTINFO>
```