To do so, we are going to run the following API, still in Kibana | Dev Tools:

![](bbox=[0.13, 0.12, 0.86, 0.5])

Figure 8.10: New mapping with the rank features field

The ingest processor has two parts:

+   - `field_map`, which maps the source text field called Message in our source index to the text_field object expected by the model.

+   - The inference configuration, which leverages the inference ELSER is applying to the data, building the collection of tokens stored in the ml.tokens field. These tokens will be used based on the search made by the user later. They are used as a semantic space to compare what the search contains.

We are now ready to reindex the data.

# Reindexing the data

We have created our ingestion process, which plays the role of a data mapper between our raw text and what the ELSER model requires to expand the text. We are using a multiple-step approach here, where you first brought the Enron dataset in via the data visualizer and created a pipeline, and are now about to reindex this data into an ELSER-formatted index. You might or might not need to go through this process, depending on whether your data exists in Elasticsearch (in which case, you would use this approach). Otherwise, you can send the data directly to this new index. With that being said,