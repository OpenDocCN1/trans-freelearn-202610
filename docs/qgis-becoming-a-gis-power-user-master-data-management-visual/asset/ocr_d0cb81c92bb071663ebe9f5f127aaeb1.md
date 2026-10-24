The layer added from these steps will give you the location of the toxic sites upstream from the chosen coordinate. If you symbolized these locations with stars and streams according to their upstream/downstream rank, you would see something similar to the following image:

![](bbox=[0.23, 0.22, 0.78, 0.71])

# The CartoDB SQL API

If you want to see the actual contents returned by a CartoDB SQL query in the JSON format, the best way to do so is by sending your SQL statement to the CartoDB SQL API endpoint at http://[YOURUSERNAME].cartodb.com/api/v2/sql. This can be useful to debug issues in interaction with your web application in particular.