> Your purchase includes a free PDF copy and exclusive extras

Your purchase includes a DRM-free PDF copy of this book, a 7-day trial to the Packt+ library (no credit card required), and additional exclusive extras. See the Free benefits with your book section in the Preface to unlock them instantly and maximize your learning.

# Extending Tableau's mapping capabilities without leaving Tableau

In our everyday lives, a map can be helpful for better understanding the world around us. For instance, maps are often used on websites, television, or in printed media to present demographic information. In such instances, the mapping requirement is static since the immediate goal does not require movement. Some businesses only have static mapping requirements—for example, a retail chain might create a visualization that includes a map to better understand sales performance in a given region. In such cases, movement between locations is not a direct need.

Often, however, a map is needed to navigate from point A to point B. This kind of mapping requirement is more complicated, because it encompasses static needs (what restaurant is nearby?) but must also deliver additional information, such as routes and distances (how can I get to that restaurant?). These dynamic mapping needs assume that movement is required to fulfill a demand. Many businesses need to understand routes and mileage, that is, how to get from point A to point B and the distances involved. These dynamic mapping requirements can vary greatly, but most of these needs share at least two things in common: routes and distances.

In the following exercise, we will consider flight routes and associated distances in Australia. Specifically, we will cover how to extract longitude and latitude numbers from Tableau and use that information in conjunction with trigonometry to calculate the mileage between various points. Along the way, we will utilize data blending, table calculations, mapping, and level of detail (LOD) calculations for a robust exercise that touches on many advanced features.

Please take the following steps:

+   1. Navigate to https://github.com/PacktPublishing/Mastering-Tableau-2026/tree/main/Chapter09 to locate and download the workbook associated with this chapter.
+   
+   2. Open the workbook and navigate to the Map worksheet.