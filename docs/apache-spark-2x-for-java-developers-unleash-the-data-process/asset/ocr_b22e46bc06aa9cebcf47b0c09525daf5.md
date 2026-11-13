the UDAF function, register to SparkSession, and finally call the alias of UDAF used while registering in SparkSession:

```
//Instantiate UDAF
AverageUDAF calcAvg= new AverageUDAF();
//Register UDAF to SparkSession
sparkSession.udf().register("calAvg", calcAvg);
//Use UDAF
sparkSession.sql("select deptno,calAvg(salary) from emp_ds group by deptno ").show(
```