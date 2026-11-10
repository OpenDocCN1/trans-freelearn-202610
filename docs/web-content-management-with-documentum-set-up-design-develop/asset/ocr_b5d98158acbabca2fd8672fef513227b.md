![](bbox=[0.19, 0.14, 0.81, 0.34])

Figure 11.2: Sample object type hierarchy in Documentum

Having understood the basics of Documentum objects, let us continue with our case study of the dummy newspaper site and design/create an object type for the center pane news article.

# 11.1 Creating Object Types

In this section, we will discuss in detail the steps for creating object types. First you need to identify the object types needed for the application. For the center pane article, we need to have one object type, say cust_newsarticle (short for custom news article) and the News Article template will be associated with this object type. In order to create a new object type, choose Insert | Object Type in the DocApp builder and specify the name cust_newsarticle under Type name as shown in figure 11.3. The new object type shows up in the checked-out mode under the Object Types folder in the left tree pane in Application Builder.