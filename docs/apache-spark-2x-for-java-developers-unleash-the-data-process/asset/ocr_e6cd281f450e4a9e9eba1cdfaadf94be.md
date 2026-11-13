# # Input sources

The following are the built-in sources available in the structured streaming library:

+   * **Socket source:** As shown in the preceding example, socket source is provided using which data can be read from a socket in UTF-8 format.
+   * **File source:** This allows us to read files placed in a directory as a stream of data. Various files types such as text, CSV, JSON, and Parquet are supported.
+   * **Kafka source:** Kafka consumer is provided to read data from Kafka broker. This is very useful for production use cases. Kafka Broker Versions 0.10.0 or higher are supported.