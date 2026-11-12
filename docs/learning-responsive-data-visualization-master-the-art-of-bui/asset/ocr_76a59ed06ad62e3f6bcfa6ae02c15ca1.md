We remark that the preceding figure looks very similar to the grid layout that we created in the beginning of this section. The only small difference in the code was the `.container` element to wrap the rows and the usage of the `.col-lg-*` classes. However, Bootstrap's grid is responsive, and the columns are arranged according to the breakpoints and the browser width. If we reduce the size of the browser window smaller than 1200px, we see that the grid jumps into a different layout. We can see this effect in the following figure:

![](bbox=[0.082, 0.203, 0.9, 0.652])

Browser output of grid layout with window size smaller than 1200px

The reason for this is that the browser window is no longer in the state of a very large device (greater than 1200px) and has jumped to a smaller state. Therefore, the `col-lg-*` columns are not applied anymore, and they appear in the full width of the row. We can now start to define certain column combinations to create a flexible responsive grid:

```
<div class="container-fluid">
    <div class="row">
        <div class="col-lg-3 col-md-6 col-sm-12"></div>
        <div class="col-lg-3 col-md-6 col-sm-12"></div>
        <div class="col-lg-3 col-md-6 col-sm-12"></div>
        <div class="col-lg-3 col-md-6 col-sm-12"></div>
```