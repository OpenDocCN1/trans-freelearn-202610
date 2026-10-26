# 3

# Managing Templates, Template Galleries, and Resources

In this chapter, we turn our attention to three main MCMS object types—templates, template galleries, and resources. We will enhance the CMS Explorer (developed in Chapter 1) to manage these objects using the MCMS Publishing API (PAPI).

# Managing Template Galleries and Templates

Out of the box, you can choose to manage template galleries and templates using either Site Manager or Template Explorer, which might be OK if you are using Visual Studio .NET as your development tool (as recommended). A compelling reason to write an administrative module to manage template galleries and templates is to work with them without having to open VS.NET. We have found this particularly useful when attempting to work in an environment where VS.NET is not installed such as on a production or a staging server. Or perhaps business requirements call for certain actions to be performed when a template gallery or template is created. Whatever the occasions may be, you may need to modify template gallery and template properties, placeholder definition and custom property collections through code.

The PAPI offers a wide range of methods and properties to work with template galleries and templates. We explore the most frequently used methods in this section, beginning with the creation of template galleries.

# Creating Template Galleries

Template galleries can be created from both Site Manager and Template Explorer. Now we will show you how we can create them from code. Let's add a Create Template Gallery dialog to the CMS Explorer interface.

+   1. Add a new web form to the project with the name CreateTemplateGallery.aspx.
+   2. In Design view, drag and drop styles.css from Solution Explorer onto the web form.
+   3. Toggle to HTML view. Insert a table with five rows. Code it in the same way as shown overleaf. A level-1 heading with the text Create Template Gallery forms the first row. The rest of the table consists of text labels and temporary markers indicating where the controls should go later.