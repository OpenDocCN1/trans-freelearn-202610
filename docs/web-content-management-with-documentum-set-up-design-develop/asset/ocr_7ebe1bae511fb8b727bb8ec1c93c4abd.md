Note that XDQL is typically used in XSL stylesheets whenever you need real-time (dynamic) data from Docbase to be displayed in web pages. In the absence of XDQL, stale data would be presented on the website via static web pages.

![](bbox=[0.26, 0.2, 0.74, 0.41])

Figure 15.15: How XDQL works

# 15.5 XDQL Example in XSL Stylesheet

Let us take a simple example to understand how XDQL is used to fetch Docbase data dynamically within Presentation files (XSL stylesheets).

The following sample template file will be transformed via an XSL stylesheet:

```
<?xml version="1.0" encoding="UTF-8"?>
<ROOTTAG>
  <TITLEOFPAGE/>
  <SHORTDESC/>
</ROOTTAG>
```

The XSL stylesheet in this example will be used to create an HTML transformation of the above template XML file. The transformed file (HTML web rendition) will display the name of the content file (object_name attribute) that is created using the above template.

Create an XSL file (as shown in figures 15.16 and 15.17) and import it within the Docbase at the following location: /Site Manager/Presentations/Editor/Custom_Presentations.