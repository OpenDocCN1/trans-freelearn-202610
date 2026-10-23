```
range, exacerbated by indigenous hunting and capture for the pet trade. Although plentiful in captivity, it is critically endangered in the wild and protected by trading prohibitions. In 2014, the species was designated a natural patrimony of Bolivia. This blue-throated macaw in flight was photographed at Loro Parque, on the Spanish island of Tenerife in the Canary Islands."
# Instantiate the summarization pipeline
summarizer = pipeline("summarization")
# Generate a summary of the text
summary = summarizer(text, max_length=100, min_length=30, do_sample=False)[0]["summary_text"]
# Print the summary
print(summary)
```

Using summarizer and the DistilBert model from Hugging Face and passing parameters such as the maximum and minimum length give us the following output:

```
The blue-throated macaw (Ara glaucogularis) is a species of macaw that is endemic to a small area of north-central Bolivia, known as the Llanos de Moxos. Recent population and range estimates suggest that about 350 to 400 individuals remain in the wild.
```

The number of models available at your fingertips is quite impressive. Communities such as Hugging Face accelerate your productivity by enabling you to run complex NLP tasks without having to even fine-tune a model.

At this point, you should have a good grasp of AI-based search, a good understanding of how to use the available techniques, and even be able to integrate some of your learning immediately into the code of your application.

# How is Elastic playing a role in this space?

Now, what role does Elastic play here? There are multiple ways you can leverage Elastic – including Elasticsearch. Elasticsearch is a distributed and highly scalable data store. Its specialty is information retrieval. It’s one thing to run the preceding code in a notebook – it’s another to make it operational at scale and to be accessed by hundreds, thousands, or millions of users.

Furthermore, training can be long as it requires the algorithm to be able to access a large amount of data, indexed for fast access, at scale. There are very few data stores that are versatile enough to be able to cope with structured and unstructured data at the same time, manage a wide range of data types, and be scalable for ingestion and search.

Elasticsearch is a unique choice, not only because of its technical attributes but also because of its vibrant community and large adoption. As mentioned earlier, the field of AI-based search applications is rapidly changing, so some consistency along the way is a luxury. Technology and communities such as Elasticsearch and Hugging Face allow users to have a safety net and some resiliency against change in the process.