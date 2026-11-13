```
})
```

To read the data, an Employee POJO object is needed, as follows:

```
public class Employee implements Serializable{
    private Integer empid;
    private String emp_name;
    private String emp_dept;
    public String toString() {
        return "Employee [empid=" + empid + ", emp_name=" + emp_name + ",
        emp_dept=" + emp_dept + "]";
    }
    public Integer getEmpid() {
        return empid;
    }
    public void setEmpid(Integer empid) {
        this.empid = empid;
    }
    public String getEmp_name() {
        return emp_name;
    }
    public void setEmp_name(String emp_name) {
        this.emp_name = emp_name;
    }
    public String getEmp_dept() {
        return emp_dept;
    }
    public void setEmp_dept(String emp_dept) {
        this.emp_dept = emp_dept;
    }
}
```

The name of the columns of the Cassandra table should exactly match the name of the fields on the class, or else it throws exceptions. Also, the object must be serializable.

To connect Spark to Cassandra, Cassandra configurations need to provide to Spark as follows:

```
SparkConf conf =new SparkConf().setMaster("local").setAppName("Cassandra Example");
conf.set("spark.cassandra.connection.host", "127.0.0.1");
JavaSparkContext jsc=new JavaSparkContext(conf);
```

Now, the rows of the Cassandra table can be mapped to the preceding POJO class using the CassandraJavaUtil library provided by spark-cassandra-connector as follows:

```
JavaRDD<Employee> cassandraRDD = CassandraJavaUtil.javaFunctions(jsc).cassandraTabl
```