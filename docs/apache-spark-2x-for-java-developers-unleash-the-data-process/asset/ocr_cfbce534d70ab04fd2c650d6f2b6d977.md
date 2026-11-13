# Feature transformers

Feature transformers are transformers that transform the data by scaling, converting, or modifying the features of parent dataframes. Some of the common feature transformers are:

+   • **Tokenizer**: Tokenizer transforms sentences or a document by splitting them into individual words. As in the following example, sentences of the dataframe will be split into words:

```
List<Row> data = Arrays.asList(
 RowFactory.create(0, "Learning Apache Spark"),
 RowFactory.create(1, "Spark has API for Java, Scala, ar
 RowFactory.create(2, "API in above laguage are very ric
 );

 StructType schema = new StructType(new StructField[]{
 new StructField("id", DataTypes.IntegerType, false, Met
 new StructField("sentence", DataTypes.StringType, false
 }});

 Dataset<Row> sentenceDataFrame = spark.createDataFrame(d&
 Tokenizer tokenizer = new Tokenizer().setInputCol("senter
 Dataset<Row> tokenized = tokenizer.transform(sentenceDat
 tokenized.show(false);
```

Tokenizer provides a very straightforward purpose of splitting sentences on the basis of whitespaces and hence at times its usability gets restricted because of its simplicity. RegexTokenizer can be used to overcome this limitation of splitting the words on the basis of regular expression.

+   • **StopWordsRemover**: This is a transformer that expunges the commonly used words from a document as they don't contribute much to the content of the document, but rather to the context such as words like is,a,the, and so on. By default, the language for stop word removal is English, however, that can be changed using the method loadDefaultStopWords (languages), where supported language are English, French, Italian, Russian, and so on. A custom list of stop words can also be specified using the stopWords