![](bbox=[0.1, 0.13, 0.9, 0.57])

Currently, the graph looks a little congested. To make the graph more intuitive, we need to add labels to the x axis and y axis, removing ticks and text from any axis to make the graph more clear. Rotating text in any axis is required when the row name or the column name contains text or a large number which is difficult to read in full length:

```
> #adding labels to the graph
> gg<-gg+labs(x="Price in Dollar", y="Carat", )
> gg
> #removing text and ticks from an axis
> gg<-gg+theme(axis.ticks.y=element_blank(),axis.text.y=element_blank())
> gg
> gg<-gg + theme(axis.text.x=element_text(angle=50, size=10, vjust=0.5))
> gg
> gg<-gg + theme(axis.text.x=element_text(color = "chocolate", vjust=0.45),
+ axis.text.y=element_text(color = "brown1", vjust=0.45))
> gg
```