+   3. Open DB Manager (**Database | DB Manager**).
+   4. Import the `sample_data` layer/shapefile.
+   5. Run the following query in the DB Manager SQL window, substituting the coordinates that you obtained in step 1, separated by a space (for example, 75.28075 39.47785):
+   ```
+   SELECT pk, calc10, min(Distance(PointFromText('POINT (- 75.28075 39.47785)'),geom)) FROM vulnerability
+   ```

Using the identify tool, click on the nearest point to the coordinate you selected to check whether the query produces the correct nearest neighbor.

# A dynamic web application – OpenLayers AJAX with Python and SpatiaLite

In this section, we will produce our web application, which, unlike any so far, involves a dynamic interaction between client and server. We will also use a different web map client API—OpenLayers. OpenLayers has long been a leader in web mapping; however, it has been overshadowed by smaller clients, such as Leaflet, as of late. With its latest incarnation, OpenLayers 3, OpenLayers has been slimmed down but still retains a functionality advantage in most areas over its newer peers.

# Server side – CGI in Python

**Common Gateway Interface (CGI)** is perhaps the simplest way to run a server-side code for dynamic web use. This makes it great for doing proof of concept learning. The most typical use of CGI is in data processing and passing it onto the database from the web forms received through HTTP POST. The most common attack vector is the SQL injection. Going a step further, dynamic processing similar to CGI is often implemented through a minimal framework, such as Bottle, CherryPy, or Flask, to handle common tasks such as routing and sometimes templating, thus making for a more secure environment.

Don't forget that Python is sensitive to indents. Indents are always expressed as spaces with a uniform number per hierarchy level. For example, an if block may contain lines prefixed by four spaces. If the if block falls within a for loop, the same lines should be prefaced by 8 spaces.