Use the nrow() function to see how much of the data was filtered from the original:

```
cat(nrow(OnlineRetail), "Original before subsetting\n")
> 268034 Original before subsetting
cat(nrow(OnlineRetail2), "After Subsetting\n")
> 55609 After Subsetting
```