
## Working with JSON data

JSON is a schemaless human-readable file format. The rise in web and mobile applications has made JSON a very popular web interchange format for API services and data storage. There are different approaches in which we can read Json files, such as reading the file as text and then processing it on a record basis, or by using SparkSession/Spark SQL, which support JSON natively.

Let's first understand how we can process JSON using records using external JSON libraries/parser with an assumption that each record in the file is of type JSON. Consider the JSON file that consists of data of people. The following is an example JSON record:

```
{ "year": "2013", "firstName": "DAVID", "county": "KINGS", "sex": "M", "cid": 272 }
```

Let's load this data as an RDD of PersonDetails object as follows:

### PersonDetails POJO:

```
public class PersonDetails implements Serializable {
private Integer cid;
private String county;
private String firstName;
private String sex;
private String year;
private Timestamp dateOfBirth;
public Integer getCid() {
return cid;
}
public void setCid(Integer cid ) {
this.cid = cid;
}
public String getCounty() {
return county;
}
public void setCounty(String county ) {
this.county = county;
}
public String getFirstName() {
return firstName;
}
public void setFirstName(String firstName ) {
this.firstName = firstName;
}
public String getSex() {
   return sex;
}
public void setSex(String sex ) {
   this.sex = sex;
}
public String getYear() {
   return year;
}
public void setYear(String year ) {
   this.year = year;
}
public Timestamp getDateOfBirth() {
   return dateOfBirth ;
}
public void setDateOfBirth(Timestamp dateOfBirth ) {
   this.dateOfBirth = dateOfBirth ;
}
}
```

### Example for working with JSON data using Java 7:

```
RDD<String>textFile=sparkSession.sparkContext().textFile("pep_json.json", 2);
JavaRDD<PersonDetails>mapParser=textFile.toJavaRDD().map(new Function<String, PersonDetails>() {
@Override
public PersonDetails call(String v1) throws Exception {
   return new ObjectMapper().readValue(v1, PersonDetails.class );
}
});
mapParser.foreach( new VoidFunction<PersonDetails>() {
   private static final long serialVersionUID = 1L;
   @Override
   public void call(PersonDetails t) throws Exception {
       System.out.println(t);
   }
});
```

### Example for working with JSON data using Java 8:

```
JavaRDD<PersonDetails>mapParser=textFile.toJavaRDD().map( v1 -> new ObjectMapper().
mapParser.foreach(t -> System.out.println(t));
```

### The code flow for parsing a JSON file is as follows:

- Read the file having JSON records, where each line must contain a separate, self-contained valid JSON object
- Once we have an RDD of String, we can use the map function to parse each line of the self-contained JSON object into an RDD of JSON class

This approach to handling a JSON object has certain benefits, such as we can filter out the data that we do not need and also the datatypes and validations on them can be done at the parsing stage itself. We also have the privilege of modifying the content, as well as erroring out data in types and formats the way we need. However, using the map function on RDD is not an optimal solution as in this case, the JSON parser object will be created for each record or line, hence one optimization can use mapPartitions() instead of the map() function, which will give access to all the records in a partition instead of one record, as in the case of the map() function.

Another approach to handling JSON is to use Spark SQL, which has the capability of handling JSON natively. Spark SQL provides automatic inference of JSON schemas for both reading and writing data. It also handles the nested fields in JSON data and provides direct access to these fields without any transformation using dataframes:

![](img/a77bd9f099223ea139d6d612f2b90712_253_0.png)

> We will discuss Spark SQL in detail in Chapter 8, Working with Spark SQL.

```
Dataset<Row> json_rec=sparkSession.read().json( "pep_json.json" );
json_rec.printSchema();

json_rec.show();
```

Considering the fact that schema definition is not mentioned in the JSON data, Spark SQL will automatically infer the schema of the JSON dataset. If the JSON fields are array then Spark creates a STRUCT of array to represent the type fields. DataFrames provides a printSchema() method that helps in visualizing the inferred schema.

If one needs to change the inferred schema then Spark provides the option to create custom schema, which can then be imposed on the JSON file that one would like to read. In the preceding dataset, timestamp is being treated as string by default, but if we want to change this default behavior, we can create a custom schema to override the defaults. One can apply the schema definition while loading the JSON file as follows:

```
StructType schema = new StructType( new StructField[] {
DataTypes.createStructField("cid", DataTypes.IntegerType, true),
DataTypes.createStructField("county", DataTypes.StringType, true),
DataTypes.createStructField("firstName", DataTypes.StringType, true),
DataTypes.createStructField("sex", DataTypes.StringType, true),
DataTypes.createStructField("year", DataTypes.StringType, true),
DataTypes.createStructField("dateOfBirth", DataTypes.TimestampType, true)
});
Dataset <Row> person_mod = sparkSession.read().schema(schema).json(textFile);
person_mod.printSchema();
person_mod.show();
```

In the previous example, the JSON field cid is long, while dateOfBirth is string if we consider the output of inferred schema. However, one can make modifications to the inferred schema by creating an explicit schema of StructType having an array of StructField with respective datatypes. An advantage of explicitly mentioning the schema of JSON is that the elements of DataFrame are not iterated twice, which will always create an overhead while parsing the JSON object. It also constrains the data with a restricted schema having specific datatypes.

RDD can be saved in JSON format as follows:

```
person_mod.write().format("json").mode("overwrite").save("pep_out.json");
```

## Working with XML Data

XML is another important file format that has extensive usage and coverage in Spark. One way to deal with XML would be to read each XML as a string of RDD and then map over each XML, parsing them using Java libraries such as DOM parser or JAXB. Though there is nothing wrong with this approach, Spark also supports a library provided by Databricks that can process a format-free XML file in a distributed way. This feature is in contrast with JSON support by Spark, where each JSON object is expected to be in-line JSON format:

```
HashMap<String, String> params = new HashMap<String, String>();
params.put("rowTag", "food");
params.put("failFast", "true");
Dataset<Row> docDF=sparkSession.read()
    .format("com.databricks.spark.xml")
    .options(params)
    .load("breakfast_menu.xml");
docDF.printSchema();
docDF.show();
```

You will notice, the similarity in the approach to handling the XML file with that of the CSV file; both libraries follow a similar structure. Some of the common features that can be used in the options() method while reading the XML files are:

- rowTag: The row tag of your XML files to treat as a row.
- samplingRatio: Sampling ratio for inferring schema (0.0 ~ 1). The default is 1.
- mode: The mode for dealing with corrupt records during parsing. The default is PERMISSIVE.
- PERMISSIVE: Sets other fields to null when it meets a corrupted record, and puts the malformed string into a new field configured by columnNameOfCorruptRecord. When a schema is set by a user, it sets null for extra fields.
- DROPMALFORMED: Ignores all corrupted records.
- FAILFAST: Throws an exception when it meets corrupted records.
- columnNameOfCorruptRecord: The name of new fields where malformed strings are stored. The default is _corrupt_record.
- valueTag: The tag used for the value when there are attributes in the element having no child. The default is _VALUE.
- charset: Defaults to UTF-8, but can be set to other valid charset names.

The inferred schema can be overridden by specifying custom schema of StructType having StructField as row elements. RDDs can be saved in XML format using the method write() over RDD, along with options as follows:

- path: Location to write files.
- rowTag: The row tag of your XML files to treat as a row.
- rootTag: The root tag of your XML files to treat as the root.
- valueTag: The tag used for the value when there are attributes in the element having no child. Default is _VALUE.
- compression: Compression codec to use when saving to file. Should be the fully qualified name of a class implementing org.apache.hadoop.io.compress.CompressionCodec or one of case-insensitive shortened names (bzip2, gzip, lz4, and snappy). Defaults to no compression when a codec is not specified:

```
docDF.write().format("com.databricks.spark.xml")
.option("rootTag", "food")
.option("rowTag", "food")
.save("newMenu.xml");
```

The pom file entry to use XML library is:

```
<dependency>
<groupId>com.databricks</groupId>
<artifactId>spark-xml_${scala.binary.version}</artifactId>
<version>0.3.<version>
</dependency>
```
