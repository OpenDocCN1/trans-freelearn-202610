# Using TFRecords for distributed learning

Although it is still not such a commonly used component of TensorFlow, the TFRecord file format is TensorFlow's own binary storage format used to store data for training our models. The binary format is an advantage when working with large datasets because it can significantly improve the performance of our processing and loading pipeline, which in turn reduces the required time to train our models. This binary data is more efficiently stored, occupies less space, and takes less time to read and write.

The TFRecord format also allows us to take advantage of multiprocessing and distributed computing features of systems like Azure Databricks, which is commonly used when dealing with large amounts of data that are too big to be stored fully in memory. In these cases, when the amount of data is too big to be stored in memory, the binary format becomes especially useful as it reduces the memory footprint of the data and the time taken to process it, and then this process is done in batches.

Another one of the features of TFRecord is that it is also optimized for storing sequential data such as time series or word embeddings, which is very useful in the case of sales trends or natural language processing applications.

In the next sections, we will learn how we can structure TFRecords, how we use them to handle large amounts of data, and how we can optimize their loading process by specifying the schema inference, all of which allows us to efficiently train our models in Azure Databricks.

# Structuring TFRecords files

The TFRecord file format is a simple record-oriented binary format that was designed with the goal of more efficiently storing data for machine and deep learning model training. The TFRecordDataset class allows us to stream files in the TFRecord format as part of our training pipeline. These TFRecord files store the data as a sequence of binary strings, which makes it necessary to predefine the structure of the data before it can be read or written from a file. To specify the structure of the data, we can use either the tf.train.Example or the tf.train.SequenceExample TensorFlow class to store samples of data, after which you must serialize it, and then it can be written to disk using TFRecordWriter.

One of the things that makes this library more optimized is that tf.train.Example is not a common Python class but a protocol buffer, which is a method to serialize structured data more efficiently.