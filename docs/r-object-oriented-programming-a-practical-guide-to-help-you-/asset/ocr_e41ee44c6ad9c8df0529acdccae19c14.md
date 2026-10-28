# # A final note

There is one final note to share about S3 classes. If you have used R, you most likely have used them. Many functions are defined to react according to the class name of their first argument. Have a look at the following diagram:

![](bbox=[0.061, 0.161, 0.665, 0.538])

A common example of this is the `plot` command. If you type the `plot` command without arguments, you can see its definition, as follows:

```
```r
> plot
function (x, y, ...)
UseMethod("plot")
<bytecode: 0x32fdd50>
<environment: namespace:graphics>
>
```
```

The `plot` command will react differently depending on what kind of object you passed to it. If you wish to see what classes the `plot` command can handle, you can use the `methods` command to list them:

```
```r
> methods(plot)
[1] plot.HoltWinters*   plot.TukeyHSD*       plot.acf*
[4] plot.data.frame*    plot.decomposed.ts*  plot.default
[7] plot.dendrogram*    plot.density*         plot.ecdf
[10] plot.factor*        plot.formula*         plot.function
[13] plot.hclust*        plot.histogram*       plot.isoreg*
[16] plot.lm*            plot.medpolish*       plot.mlm*
[19] plot.ppr*           plot.prcomp*          plot.princomp*
```
```