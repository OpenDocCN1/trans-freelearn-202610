# # Loading multiple resources

As you have already learned in the previous section, we always have to load the geometry of the background map in addition to the file containing the data that we want to display. This is a little challenging because now we have to wait until these (at least) two files have loaded before we can draw the visualization.

Ideally, we want to load both resources in parallel and execute a callback function once all the resources are available. Luckily, Mike Bostock was very active in the past years and wrote the utility tool Queue.js which does exactly this.

You can install Queue.js using bower and run the following command from the command line or load Queue.js from a CDN:

```
bower install queue-async
```

Now, we can create a queue and use the .defer() function to register functions to load all the resources. We pass a function as the first argument, and Queue.js will automatically pass all additional arguments to this function. Finally, we call the .await() method to execute a callback once all the resources have loaded and collect all their values. Let's look at an example:

```
queue()
.defer(d3.text, "path/to/file/file1.txt")
.defer(d3.json, "path/to/file/file2.json")
.defer(d3.csv, "path/to/file/file3.csv")
.await(function(err, file1, file2, file3) {
 console.log(file1, file2, file3);
});
```

In the preceding code, we see how easy it is now to deal with multiple resources at once—super easy, thanks to Mike Bostock.

> More information about queue.js can be found on the Github page of the project at https://github.com/mbostock/queue.

# # Types of geographic visualization

According to the values and quantities that we want to display, we can choose from various visualizations that can best represent the data. Usually, we choose between line charts, bar charts, area charts (normal or stacked), pie charts, scatter plots, and so on.