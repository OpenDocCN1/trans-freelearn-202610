# Working with Databases

```
```java
js.jobs.add(job);
job = new Job("Tester", 4500, "Mozilla", new DateTime.now());
js.jobs.add(job);
// 2- opening the database:
js.open();
// 3- storing data in database:
js.storeData();
// 4- retrieving and displaying data from database:
js.readData();
}
```
```

After running the preceding code, we can verify that the insertions succeeded in MySQL Workbench in the `jobs` table:

![](bbox=[0.18, 0.354, 0.819, 0.619])

The readData method reads the data from the **jobs** table and prints it to the console:

**dbKey: 1, type : Web Developer, salary: 7500, company: Google, posted: 2014-07-02 14:34:12.000, open: 1**

**dbKey: 2, type : Software Engineer, salary: 5500, company: Microsoft, posted: 2014-07-02 14:34:12.000, open: 1**

**dbKey: 3, type : Tester, salary: 4500, company: Mozilla, posted: 2014-07-02 14:34:12.000, open: 1**