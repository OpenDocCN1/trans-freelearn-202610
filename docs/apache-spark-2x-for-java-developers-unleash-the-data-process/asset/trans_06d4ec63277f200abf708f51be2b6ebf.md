

## Java 8：

```
JavaRDD<Person> peoplePart = textFile.mapPartitions(new FlatMapFunction<Iterator<String>, Iterator<Person>>() {
    @Override
    public Iterator<Person> call(Iterator<String> p) throws Exception {
        ArrayList<Person> personList = new ArrayList<Person>();
        while (p.hasNext()) {
            String[] parts = p.next().split("~");
            Person person = new Person();
            person.setName(parts[0]);
            person.setAge(Integer.parseInt(parts[1].trim()));
            person.setOccupation(parts[2]);
            personList.add(person);
        }
        return personList.iterator();
    }
});
peoplePart.foreach(new VoidFunction<Person>() {
    @Override
    public void call(Person p) throws Exception {
        System.out.println(p);
    }
});
```

```
JavaRDD<Person> peoplePart = textFile.mapPartitions(p -> {
    ArrayList<Person> personList = new ArrayList<Person>();
    while (p.hasNext()) {
        String[] parts = p.next().split("~");
        Person person = new Person();
        person.setName(parts[0]);
        person.setAge(Integer.parseInt(parts[1].trim()));
        person.setOccupation(parts[2]);
        personList.add(person);
    }
    return personList.iterator();
});
peoplePart.foreach(p -> System.out.println(p));
```

同样，以特定格式保存文本文件也可以通过对 RDD 调用 saveAsTextFile() 方法以类似方式完成。它会将数据写入文本文件，对每个 RDD 元素调用 .toString() 方法，每行写入一个元素：

```
people.saveAsTextFile("Path of output file");
```

输出文件的数量等于 RDD 中的分区数量，因此可以在保存文本文件之前，通过对 RDD 调用带适当数量的 repartition() 方法来控制：

```
people.repartition(1).saveAsTextFile("Path of output file");
```

虽然 repartition() 方法在保存时能实现管理输出文件的任务，但由于数据混洗，这可能是一个代价高昂的过程。Coalesce 是另一个可选方案，可以代替 repartition() 使用，它会尽量避免数据混洗，因此可能更适合此类操作：

```
people.coalesce(1).saveAsTextFile("Path of output file");
```

> 请参阅第 7 章 Spark 编程模型 – 高级 中的“高级 Spark 转换”部分，以详细了解 repartitioning 和 coalesce 转换的工作原理。

## 使用 CSV 数据

CSV 文件可以视为纯文本文件，以逗号作为分隔符，通常会按预期工作。假设我们有以下格式的电影数据：

```
movieId, title, genre
```

让我们将此文件加载为 Movie 对象的 RDD，如下所示：

### Movie POJO：

```
public class Movie implements Serializable {
    private Integer movieId;
    private String title;
    private String genre;

    public Movie() {
    };

    public Movie(Integer movieId, String title, String genre) {
        super();
        this.movieId = movieId;
        this.title = title;
        this.genre = genre;
    }

    public Integer getMovieId() {
        return movieId;
    }

    public void setMovieId(Integer movieId) {
        this.movieId = movieId;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getGenre() {
        return genre;
    }

    public void setGenre(String genre) {
        this.genre = genre;
    }

    public static Movie parseRating(String str) {
        String[] fields = str.split(",");
        if (fields.length != 3) {
            System.out.println("The elements are ::");
            Stream.of(fields).forEach(System.out::println);
            throw new IllegalArgumentException("Each line must contain 3 fields while the current line has ::" + fields.length);
        }
        Integer movieId = Integer.parseInt(fields[0]);
        String title = fields[1].trim();
        String genre = fields[2].trim();
        return new Movie(movieId, title, genre);
    }
}
```

## Spark 代码：

### 使用 Java 7 处理 CSV 数据的示例：

```
JavaRDD<Movie> moviesRDD = sparkSession.read().textFile("movies.csv")
    .javaRDD().filter(str -> !(null == str))
    .filter(str -> !(str.length() == 0))
    .filter(str -> !str.contains("movieId"))
    .map(new Function<String, Movie>() {
        private static final long serialVersionUID = 1L;

        public Movie call(String str) {
            return Movie.parseRating(str);
        }
    });
moviesRDD.foreach(new VoidFunction<Movie>() {
    @Override
    public void call(Movie m) throws Exception {
        System.out.println(m);
    }
});
```

### 使用 Java 8 处理 CSV 数据的示例：

```
JavaRDD<Movie> moviesRDD = sparkSession.read().textFile("movies.csv")
    .javaRDD().filter(str -> !(null == str))
    .filter(str -> !(str.length() == 0))
    .filter(str -> !str.contains("movieId"))
    .map(str -> Movie.parseRating(str));
moviesRDD.foreach(m -> System.out.println(m));
```

但是，在某些情况下，例如字段内包含逗号，如果不显式处理此类情况就会失败；或者在某些情况下，人们可能希望推断 CSV 的架构，甚至获取头信息。对于所有此类场景，都可以使用由 **Databricks** 提供的 Spark CSV 库来处理 CSV 数据类型：

```
Dataset<Row> csv_read = sparkSession.read().format("com.databricks.spark.csv")
    .option("header", "true")
    .option("inferSchema", "true")
    .load("movies.csv");
csv_read.printSchema();
csv_read.show();
```

可以通过指定具有 StructField 作为元素的 StructType 架构来覆盖默认推断的架构，以表示 CSV 字段：

```
StructType customSchema = new StructType(new StructField[] {
    new StructField("movieId", DataTypes.LongType, true, Metadata.empty()),
    new StructField("title", DataTypes.StringType, true, Metadata.empty()),
    new StructField("genres", DataTypes.StringType, true, Metadata.empty())
});

Dataset<Row> csv_custom_read = sparkSession.read().format("com.databricks.spark.csv")
    .option("header", "true")
    .schema(customSchema)
    .load("movies.csv");
csv_custom_read.printSchema();
csv_custom_read.show();
```

读取 CSV 文件时可以在 options() 方法中使用的一些功能如下：

- **Header**：设置为 true 时，文件的第一行将用于命名列，且不会包含在数据中。所有类型都将被假定为字符串。默认值为 false。
- **Delimiter**：默认情况下，列使用 "," 分隔，但分隔符可以设置为任何字符。
- **Quote**：默认情况下，引号字符为 "，但可以设置为任何字符。引号内的分隔符会被忽略。
- **Escape**：默认情况下，转义字符为 \，但可以设置为任何字符。转义的引号字符会被忽略。
- **Mode**：确定解析模式。默认值为 Permissive。可能的值为：
  - **PERMISSIVE**：尝试解析所有行：缺失的标记将插入 null，额外的标记将被忽略。
  - **DROPMALFORMED**：删除标记数少于或多于预期的行，或标记与架构不匹配的行。
  - **FAILFAST**：如果遇到任何格式错误的行，则以 RuntimeException 中止。
- **Charset**：默认为 UTF-8，但可以设置为其他有效的字符集名称。
- **InferSchema**：自动推断列类型。它需要对数据进行一次额外遍历，默认值为 false。

可以使用 RDD 上的 `write()` 方法将数据保存到磁盘，并伴随其格式和压缩编解码器（如果有），如下所示：

```
csv_custom_read.write()
    .format("com.databricks.spark.csv")
    .option("header", "true")
    .option("codec", "org.apache.hadoop.io.compress.GzipCodec")
    .save("newMovies.csv");
```

写入 CSV 文件时可以在 `options()` 方法中使用的一些常用功能如下：

- **Header**：设置为 true 时，头信息（来自 DataFrame 中的架构）将写入第一行。
- **Delimiter**：默认情况下，列使用 `,` 分隔，但分隔符可以设置为任何字符。
- **Quote**：默认情况下，引号字符为 `"`，但可以设置为任何字符。这将根据 `quoteMode` 写入。
- **Escape**：默认情况下，转义字符为 `\\`，但可以设置为任何字符。转义的引号字符将被写入。
- **Codec**：保存到文件时要使用的压缩编解码器。应为实现 `org.apache.hadoop.io.compress.CompressionCodec` 的类的完全限定名称，或不区分大小写的缩写名称之一（bzip2、gzip、lz4 和 snappy）。如果未指定编解码器，默认不启用压缩。

除了逗号 (,)，该库还可以通过使用分隔符选项指定该字符，来读取任何字符分隔的文件。

