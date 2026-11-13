features one is supposed to set features name firsthand by using the setNames() method:

```
Dataset<Row> featSet = emp_ds.select(col("empId"),col("salary"),col("deptn
String[] featuresCols = featSet.columns();
Dataset<Row> vecAssembler = new VectorAssembler().setIn
VectorSlicer vectorSlicer = new VectorSlicer().setInputCo
vectorSlicer.setIndices(new int[]{1, 2});
Dataset<Row> output = vectorSlicer.transform(vecAssembler
output.show(false);
```