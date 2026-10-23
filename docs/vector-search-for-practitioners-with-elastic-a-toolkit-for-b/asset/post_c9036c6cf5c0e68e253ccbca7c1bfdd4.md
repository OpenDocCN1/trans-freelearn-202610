
Now that we’ve grasped the importance of recognizing the sources of PII and implementing rigorous security measures, the next logical step is to discuss the practical techniques for redacting such sensitive information. In the digital age, where structured and unstructured data often mingle, a blended approach using both NER models and regex patterns emerges as a robust solution. Let’s explore how these tools can be harnessed effectively to safeguard PII.

## Redacting PII with NER models and regex patterns

The process of redacting PII often requires a multi-faceted approach to ensure that sensitive data is accurately identified and removed from various data sources. Two key techniques used to redact PII are **Named Entity Recognition (NER)** models and **regular expressions (regex)** patterns. Combining these methods can help identify a broad range of PII types and ensure comprehensive data protection.

## NER models

NER is a **Natural Language Processing (NLP)** technique used to identify and classify named entities, such as names of people, locations, organizations, and other specific information within text data. NER models can be particularly useful in redacting PII, as they can identify entities that do not follow a common pattern or structure. This enables the detection and redaction of less predictable PII types, which may be more difficult to identify using regex patterns alone.

Machine learning models, such as those based on **BERT (Bidirectional Encoder Representations from Transformers)**, have demonstrated significant success in NER tasks. One such model is **dslim/bert-base-NER** from Hugging Face, which has been trained to identify a variety of named entities. By leveraging NER models such as these, organizations can efficiently identify and redact PII present in unstructured text data, such as emails, documents, and logs.

## Regex patterns

Regex are a powerful tool for pattern matching and manipulation within text data. Regex patterns are particularly useful for identifying and redacting PII that follows standard patterns or structures. By designing regex patterns to match these common structures, organizations can quickly and accurately detect and remove PII from various data sources.

**Grok patterns** offer a more user-friendly alternative to regex for pattern matching in text data. Grok patterns are essentially a collection of predefined regex patterns, which can be combined and reused to create more complex expressions with greater ease. This approach simplifies the process of writing and maintaining pattern-matching rules, making it more accessible to users who may not be well-versed in regex syntax.

For the redaction pipeline in Elasticsearch, we will be using a new processor called the **redact processor**. This processor utilizes Grok patterns rather than traditional regex for pattern matching. In the following section, we will discuss the implementation and usage of the redact processor in more detail, outlining how it leverages Grok patterns to identify and redact PII from data sources.

Examples of PII types that can be identified using regex/grok patterns include the following:

- **Credit card numbers**: Credit card numbers typically follow a specific pattern, with a fixed number of digits and recognizable formatting. Regex patterns can be designed to identify and redact these numbers, regardless of the card issuer.
- **Social Security numbers**: Social Security numbers in the United States consist of nine digits, separated into three groups by hyphens (e.g., 123-45-6789). This standard format makes it straightforward to create a regex pattern to identify and redact these numbers.
- **Bank account numbers**: While bank account numbers can vary in length and formatting depending on the financial institution, many banks follow a common pattern that can be identified using regex patterns. Additionally, the presence of specific keywords, such as “account number” or “routing number,” can help refine regex patterns to accurately identify and redact this information.
- **Email addresses**: Email addresses follow a standard format with a local part, an “@” symbol, and a domain. Regex patterns can be designed to identify and redact email addresses, helping to protect the privacy of individuals and prevent potential data breaches.
- **Phone numbers**: Phone numbers often follow a recognizable pattern, with a fixed number of digits and standard formatting (e.g., (123) 456-7890 or +1 123 456 7890). Regex patterns can be used to identify and redact phone numbers from various data sources, ensuring that contact information remains private.

While grok patterns excel at recognizing structured data forms such as credit card numbers or email addresses, there remain subtle nuances in unstructured data that might elude them. Herein lies the value of merging grok with NER models. By integrating Grok’s precision with the contextual depth of NER, we aim to create a more encompassing redaction strategy, significantly reducing the chances of PII going unnoticed. Let’s explore this combined approach for more robust data protection.

## Combining NER models and regex (or grok) patterns for PII redaction

To maximize the effectiveness of PII redaction, organizations should consider using a combination of NER models and regex patterns. This two-stage approach ensures that both structured and unstructured PII can be accurately identified and removed from data sources.

In the first stage, NER models can be applied to text data to identify named entities that may contain PII. The output from the NER models can then be used to redact or mask the identified entities within the text.

Having discussed the synergy of NER models with regex for spotting PII, let’s discuss how to operationalize a PII redaction pipeline in Elasticsearch. The Elasticsearch platform offers a powerful solution with ingest pipelines, designed to scrub data upon ingestion. Melding the precision of grok patterns with our discussed techniques provides a more holistic approach to sensitive data handling.

## PII redaction pipeline in Elasticsearch

The PII redaction pipeline in Elasticsearch aims to automatically redact sensitive information from data as it's ingested into the Elasticsearch cluster. This process ensures that sensitive data is protected, which is particularly important when handling personal information that could be used to identify an individual, such as names, addresses, phone numbers, and social security numbers.

In this section, we will discuss the steps users can take to configure the PII redaction pipeline in Elasticsearch.

For the complete code, open the Jupyter Notebook in the chapter 6 folder of the book's GitHub repository: https://github.com/PacktPublishing/Vector-Search-for-Practitioners-with-Elastic/tree/main/chapter6.

We will review the key points of the pipeline.

## Generating synthetic PII

To run our pipeline, we will need a dataset. Thankfully we have faker, the Python library for generating fake data of a given type. Our task here involves two steps:

- Generating the data with faker
- Bulk-indexing the data in Elasticsearch

The following is the code to generate the data (you can use the pip install command to install Faker if not already installed):

```
from faker import Faker
import json
from pprint import pprint

# Create an instance of the Faker class
fake = Faker()

# Define a function to generate fake personal identification information
def generate_fake_pii(num_records):

fake_data = []

for x in range(num_records):
# Generate fake personal identification information
fn = fake.first_name()
ln = fake.last_name()
pn = fake.phone_number()
```

```
sn = fake.ssn()
ai = fake.random_element(elements=('active', 'inactive'))

call_log = {
'message' : f'{fn} {ln} called in from {pn}. Their account number is {sn}',
'status' : ai
}

fake_data.append(call_log)
return fake_data

# Generate fake personal identification information for N individuals
num_records = 10 # Set the desired number of records
fake_pii_data = generate_fake_pii(num_records)

pprint(fake_pii_data)
```

This will output records that look like the following:

```
{
'message': 'Stephen Gomez called in from 9162417050. Their account number is ' '275-51-8055',
'status': 'inactive'
},
# Import the required libraries
from faker import Faker
import json
# Create an instance of the Faker class
fake = Faker()
# Define a function to generate fake personal identification information
def generate_fake_pii(num_records):
# Create an empty list to store the fake data
fake_data = []
# Generate the specified number of fake records
for _ in range(num_records):
# Generate fake personal identification information
person = {
'first_name': fake.first_name(),
'last_name': fake.last_name(),
'email': fake.email(),
'phone_number': fake.phone_number(),
```
