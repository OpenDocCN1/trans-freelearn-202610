
# Performing the bulk index operation

With our bulk_index_body array ready, we can now perform the bulk index operation:

```
try:
    response = helpers.bulk(es, bulk_index_body, chunk_size=500,
    request_timeout=60*3)
    print ("\nRESPONSE:", response)
except BulkIndexError as e:
    for error in e.errors:
        print(f"Document ID: {error['index']['_id']}")
        print(f"Error Type: {error['index']['error']['type']}")
        print(f"Error Reason: {error['index']['error']['reason']}")
```

We use the helpers.bulk() function from the Elasticsearch library to provide our Elasticsearch connection (es)—the bulk_index_body array we just created—with a chunk_size value of 500 (which specifies that we want to send 500 documents per request) and a request_timeout value of 180 seconds, which specifies that we want to allow each request to take up to 3 minutes before timing out because the indexing could take a long time with ELSER.

The helpers.bulk() function will return a response indicating the number of operations attempted and the number of errors, if any.

If any errors occur during the bulk index operation, these will be raised as BulkIndexError. We can catch this exception and iterate over its errors attribute to get information about each individual error, including the ID of the document that caused the error, the type of error, and the reason for it.

At the end of this process, you will have successfully indexed your entire Allrecipes.com dataset in Elasticsearch, ready for it to be retrieved and processed by your RAG-enhanced CookBot.

## Building the retriever—RRF with ELSER

In the context of our recipe retrieval task, our goal is to maximize the relevance of the returned recipes based on a user's query. We will utilize a combination of classic full-text search (via BM25), semantic search (with ELSER), and a robust rank fusion method (RRF). This combination allows us to handle more complex queries and return results that align closely with the user's intent.

Let's consider the following query:

```
GET recipes/_search
{
  "_source": { "includes": [ "name", "ingredient" ] },
  "sub_searches": [
    {
      "query": {
        "bool": {
```

```
"must": { "match": { "ingredient": "carrot beef" } },
"must_not": { "match": { "ingredient": "onion" } }
},
{ "query": { "text_expansion": { "ml.tokens": { "model_id": ".elser_model_1", "model_text": "I want a recipe from the US west coast with beef" } } } },
"rank": { "rrf": { "window_size": 50, "rank_constant": 20 } }
```

This query includes two types of search. The first uses a classic Elasticsearch Boolean search to find recipes that contain both carrot and beef as ingredients, excluding those with onion. This traditional approach ensures that the most basic constraints of the user are met.

The second sub_search employs ELSER to semantically expand the query I want a recipe from the US west coast with beef. ELSER interprets this request based on its understanding of language, enabling the system to match documents that may not contain the exact phrase but are contextually related. This allows the system to factor in the more nuanced preferences of the user.

The query then employs RRF to combine the results of the two sub_searches. The window_size parameter is set to 50, meaning the top 50 results from each sub-search are considered. The rank_constant parameter, set to 20, guides the RRF algorithm to fuse the scores from the two sub_searches.

Thus, this query exemplifies the effective combination of BM25, ELSER, and RRF. Exploiting the strengths of each allows CookBot to move beyond simple keyword matches and provide more contextually relevant recipes, improving the user experience and increasing the system's overall utility.

## Leveraging the retriever and implementing the generator

Now that we have our Elasticsearch retriever set up and ready to go, let's proceed with the final part of our RAG system: the generator. In the context of our application, we'll use the GPT-4 model as the generator. We'll implement the generator in our recipe_generator.py module and then integrate it into our Streamlit application.

### Building the generator

We will start by creating a RecipeGenerator class. This class is initialized with an OpenAI API key (find out how to get an OpenAI key at https://help.openai.com/en/articles/4936850-where-do-i-find-my-secret-api-key), which is used to authenticate our requests with the GPT-4 model:

```
import openai
import json
from config import OPENAI_API_KEY

class RecipeGenerator:
    def __init__(self, api_key):
        self.api_key = api_key
        openai.api_key = self.api_key
```

Next, we define the generate function in the RecipeGenerator class.
This function takes in a recipe as input, and sends it as a prompt to
the GPT-4 model, asking it to generate a detailed, step-by-step guide.

```
def generate(self, recipe):
    prompts = [{"role": "user", "content": json.dumps(recipe)}]
    instruction = {
        "role": "system",
        "content": "Take the recipes information and generate a recipe with a mouthwatering intro and a step by step guide."
    }
    prompts.append(instruction)

    generated_content = openai.ChatCompletion.create(
        model="gpt-4",
        messages=prompts,
        max_tokens=1000
    )
    return generated_content.choices[0].message.content
```

The prompts are formatted as required by the OpenAI API, and the max_tokens parameter is set to 1000 to limit the length of the generated text. The generated recipe is then returned by the function.

## Integrating the generator into the Streamlit application

With our RecipeGenerator class ready, we can now integrate it into our Streamlit application in main.py. After importing the necessary modules and initializing the RecipeGenerator class, we will set up the user interface with a text input field:

```
from recipe_generator import RecipeGenerator
from config import OPENAI_API_KEY
generator = RecipeGenerator(OPENAI_API_KEY)
input_text = st.text_input(" ", placeholder="Ask me anything about cooking")
```

When the user enters a query, we will use the Elasticsearch retriever to get a relevant recipe. We then pass this recipe to the generate function of RecipeGenerator, and the resulting text is displayed in the Streamlit application (see a video example at https://www.linkedin.com/posts/bahaaldine_genai-gpt4-elasticsearch-activity-7091802199315394560-TkPY):

```
if input_text:
    query = {
        "sub_searches": [
            {
                "query": {
                    "bool": {
                        "must_not": [
                            {
                                "match": {
                                    "ingredient": "onion"
                                }
                            }
                        ]
                    }
                }
            },
            {
                "query": {
                    "text_expansion": {
                        "ml.tokens": {
                            "model_id": ".elser_model_1",
                            "model_text": input_text
                        }
                    }
                }
            }
        ],
        "rank": {
            "rrf": {
```

```
"window_size": 50,
"rank_constant": 20
    }
}
recipe = elasticsearch_query(query)
st.write(recipe)
st.write(generator.generate(recipe))
```

The generator thus works in tandem with the retriever to provide a detailed, step-by-step recipe based on the user’s query. This completes our implementation of the RAG system in a Streamlit application, bridging the gap between retrieving relevant information and generating coherent, meaningful responses.

## Summary

After a deep exploration of the numerous technicalities and advanced methods involved in building CookBot, we can conclude that the application of RAG, ELSER, BM25, and RRF has significantly contributed to CookBot’s unique ability to answer culinary queries with enhanced precision and depth.

Throughout the course of this chapter, we’ve uncovered the potential of RAG as a retriever for finding relevant documents and as a generator for crafting detailed responses. By incorporating ELSER and BM25, the retrieval component gains the advantage of both semantic context and keyword efficiency. The fusion of these retrieval methods with RRF leads to the curation of a highly relevant set of recipes, even when faced with complex or vague queries.

The integration of RAG into CookBot’s architecture has further amplified its capabilities, demonstrating the value of an iterative approach where knowledge is refined over multiple steps. By employing GPT-4 as the generator in the RAG setup, CookBot is capable of producing diverse, rich, and contextually accurate responses.

With the utilization of the Allrecipes.com dataset as the base and the well-coordinated use of RAG, ELSER, BM25, and RRF, we have managed to create CookBot, a bot that can expertly navigate the culinary space and provide personalized assistance. This model serves as an illustration of how combining state-of-the-art retrieval methods with powerful language models can yield practical and efficient solutions for diverse domains.

As we move forward, we can anticipate further enhancements and refinements to CookBot, demonstrating the endless possibilities in the realm of AI-driven culinary assistance. This exploration has served as a primer for us to dive into more advanced techniques and strategies that can be employed to create powerful conversational AI systems.

We hope you have gained a comprehensive understanding of these key concepts and their applications to gain a deeper appreciation for the fascinating field of AI. In the next chapter, we will tackle the ChatGPT plugin framework and extend our acquired knowledge about RAG to integrate directly into the ChatGPT experience.
