feature_description is a dictionary that holds the description of the features, which is necessary for our parsing function.

4. Here, our next step will define the parsing function, which will read the tf.train.Example class input using the previously defined feature_description dictionary:

```
def _parsing_function(proto_input):
    return tf.io.parse_single_example(proto_input,
    feature_description)
```

5. As the next step to read our TFRecord files, we will apply the parsing function that we have defined to each item in the dataset using the tf.data.Dataset.map method:

```
parsed_dataset = raw_dataset.map(_parsing_function)
parsed_dataset
```

We can also parse batches of data using the tf.io.parse_example function as an alternative to reading them one by one.

6. We can examine the data by displaying the observations in the dataset when eager execution is enabled. Here, regardless of the number of observations in the dataset, we will only display the first 10 of them, which will be shown as a dictionary of features:

```
for parsed_record in parsed_dataset.take(10):
    print(repr(parsed_record))
```

Each item shown is a tf.Tensor, which is a TensorFlow class that has as a value the numpy element of this tensor.

7. To write observations as a TFRecord file, we need to transform each observation to a tf.train.Example class before it is written to file. Here, we will write n_obs number of tf.train.Example records to a TFRecord file:

```
with tf.io.TFRecordWriter(filename) as writer:
    for i in range(n_obs):
        example = serialize_example(feature0[i],
                                    feature1[i],
                                    feature2[i])
        writer.write(example)
```