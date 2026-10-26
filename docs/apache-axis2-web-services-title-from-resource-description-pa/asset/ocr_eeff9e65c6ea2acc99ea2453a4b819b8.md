# 3

# Axis 2 XML Model (AXIOM)

AXIOM stands for Axis2 Object Model and refers to the XML InfoSet model that was initially developed as part of Apache Axis2, but later it moved as a WS commons project so that projects other than Axis2 were able to benefit from it. XML InfoSet refers to the information included inside the XML, and for programmatic manipulation it is convenient to have a representation of this XML InfoSet in a language-specific manner. For an object-oriented language, the obvious choice is a model made up of objects. DOM and JDOM are two such XML models. AXIOM is conceptually similar to such an XML model by its external behavior, but deep down it is very much different. At the end of this chapter, you will understand the basics of AXIOM and the best practices to be followed while using AXIOM.

This chapter will cover:

+   - AXIOM architecture
+   - Pull parsing
+   - Working with AXIOM
+   - Advanced operations with AXIOM

# Overview of AXIOM and its features

Apache Axiom was designed to provide fast and better XML, which became the heart of any given XML-processing system. Axiom is a lightweight implementation built on deferred parsing technology. More concretely, it is an implementation of StAX (JSR 173), the standard streaming pull parser API. One of the main intentions of this was to provide a standard manipulation API. Thus the AXIOM object model can be manipulated flexibly as any other object model (for example, JDOM), but underneath, the objects will be created only when they are absolutely required (that is, on-demand building). This leads to less memory-intensive programming.