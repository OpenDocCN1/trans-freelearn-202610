```
long newCount = buffer.getCountVal() + 1;
buffer.setSumVal(newSum);
buffer.setCountVal(newCount);
return buffer;
}
public Average merge(Average b1, Average b2) {
double mergedSum = b1.getSumVal() + b2.getSumVal();
long mergedCount = b1.getCountVal() + b2.getCountVal();
b1.setSumVal(mergedSum);
b1.setCountVal(mergedCount);
return b1;
}
public Double finish(Average reduction) {
return ((double) reduction.getSumVal()) / reduction.getCountVal();
}
public Encoder<Average> bufferEncoder() {
return Encoders.bean(Average.class);
}
public Encoder<Double> outputEncoder() {
return Encoders.DOUBLE();
}
}
```

Instantiate the UDAF class, convert the instance to a typed column giving it an alias as the column name, and then pass it to the dataset:

```
TypeSafeUDAF typeSafeUDAF=new TypeSafeUDAF();
Dataset<Employee> emf = emp_ds.as(Encoders.bean(Employeee.class));
TypedColumn<Employeee, Double> averageSalary = typeSafeUDAF.toColumn().name("average
Dataset<Double> result = emf.select(averageSalary);
```