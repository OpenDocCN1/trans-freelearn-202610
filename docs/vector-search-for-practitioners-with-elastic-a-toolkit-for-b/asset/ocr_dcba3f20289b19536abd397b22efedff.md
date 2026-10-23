This is what the Embedchain bot output looks like for this example:

```
elastic_bot.query("Tell me about the Customer Architect role at Elastic")
```

'The Customer Architect role at Elastic is a position within the Customer Success Group. Customer Architects at Elastic are responsible for working with customers to understand their needs and requirements, and then designing and implementing solutions using Elastic's technology stack. They help customers optimize their use of Elastic's products for search, observability, and security purposes. Customer Architects also provide technical guidance and support to customers throughout the implementation process.'

Figure 10.12: Embedchain output

It is as simple as that! So, let’s put this into the context of our goal. We want the user to interact through ChatGPT with our plugin that, in response to a prompt, will load the correct documentation page via Embedchain. When an Embedchain application adds content, it actually stores it in an underlying vector database. It uses OpenAI’s Ada embedding model to create the embeddings out of the chunk of data that it scraped. All the chunking and hydration of the data is done by Embedchain.

In this example, we will use Elasticsearch as a vector database. The following code block shows the structure of the document indexed by Embedchain when a link is added:

```
{
    "_index": "embedchain_store_1536",
    "_id": "635ef64b5a99725d3b2cdf1cca86d5570a70388aa4d8d328cb2a02dd74b232c8",
    "_score": 1,
    "_source": {
        "text": "Aligning with customer business goals, you'll also guide them through onboarding, ensure they adopt our solutions, and aid in their growth. As a Customer Architect, you'll stand as the Go-to Technical Counsel, collaborating closely with the customer and partnering with teams like Field and Services. Your role ensures that Elastic's offerings consistently surpass customer hopes. Leveraging your technical knowledge, business acumen, and commitment to value, you'll enhance and elevate the customer journey.",
        "metadata": {
            "url": "https://jobs.elastic.co/jobs/csm/united-states/customer-architect/5112021?gh_jid=5112021#",
            "data_type": "web_page",
            "hash": "832c08a036185237195ac37a6133db16"
        },
        "embeddings": [...] 
    }
}
```