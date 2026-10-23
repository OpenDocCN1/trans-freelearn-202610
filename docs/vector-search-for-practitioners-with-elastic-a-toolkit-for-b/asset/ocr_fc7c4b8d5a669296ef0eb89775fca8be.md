# Select an ingestion method

Create a search optimized Elasticsearch index to store your content. Start by selecting an ingestion method.

Figure 10.7: Ingestion method selection

6. Name your index—here, I used the name packt-cdl-source:

# Web crawler search index

Use the web crawler to programmatically discover, extract, and index searchable content from websites and knowledge bases.

# Create an Elasticsearch index

This index will hold your data source content, and is optimized with default field mappings for relevant search experiences. Give your index a unique name and optionally set a default language analyzer for the index.

Figure 10.8: Web crawler search index creation

7. In the Domain URL field, add https://www.elastic.co, as this is the one we want to crawl: