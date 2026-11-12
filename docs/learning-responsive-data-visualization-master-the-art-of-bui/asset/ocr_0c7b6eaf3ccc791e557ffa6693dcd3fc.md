# Using Bootstrap's grid system

Another great and quick way to make your charts responsive and play nicely together with Bootstrap is to integrate them into Bootstrap's grid system. The best and cleanest integration, however, is to separate concerns and make the visualization as general and adaptive as possible.

Let's take our bar chart example with the custom resize events and integrate it into a simple grid layout. As usual, you can find the complete source code of the example in the code examples:

```
<div class="container">
 <div class="row">
 <div class="col-md-8">
 <div class="chart" data-url="..." ...>
 </div>
 </div>
 <div class="col-md-4">
 <h2>My Dashboard</h2>
 <p>This is a simple dashboard</p>
 </div>
 </div>
 <div class="row">
 <div class="col-md-4">
 <div class="chart" data-url="..." ...>
 </div>
 </div>
 <div class="col-md-4">
 <div class="chart" data-url="..." ...>
 </div>
 </div>
 <div class="col-md-4">
 <div class="chart" data-url="..." ...>
 </div>
 </div>
 </div>
 </div>
 <div class="row">
 <div class="col-md-6">
 <div class="chart" data-url="..." ...>
 </div>
 </div>
 <div class="col-md-6">
 <div class="chart" data-url="..." ...>
 </div>
 </div>
 </div>
 </div>
```