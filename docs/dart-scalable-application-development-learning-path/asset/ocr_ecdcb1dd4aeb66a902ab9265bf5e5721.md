# # Summary

The applications created in this chapter have been numerous, but all work together to reach the goal of a live display of data.

The data collector transforms raw live data into a processable form. The REST API was created with the rpc package, taking much of the work out of the way. Creating a new web API method is a case of adding an annotated regular Dart method.

The consuming web application showed how to connect to a JSON standard web service and produce high-quality output, updated in real time. The HTML DOM was used to swap in a new table-based display.

The incoming data on earthquakes has proven quite fascinating to watch, but it could be more visual. In the next chapter, we’ll create a visualization using the feature geometry data stashed in the database, and look at expanding the API to do more than provide a data feed.