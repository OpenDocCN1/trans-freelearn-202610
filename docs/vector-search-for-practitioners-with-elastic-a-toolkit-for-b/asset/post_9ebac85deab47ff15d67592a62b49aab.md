
Conversational Engine (CE) 190
Convolutional Neural Networks (CNNs) 94
cosine similarity 32, 95
curse of dimensionality
  reference link 70
cybersecurity 20

## D

data extraction 191
data
  reindexing 158-160
data type
  boolean data type 22
  date 22
  geo data type 22
  impact on relevancy 22
  keyword data type 22
  numeric types 22
  text data type 22
data types, containing PII
  APM data 106
  databases and data stores 106
  logs 106
  unstructured data 106
dense vector 36, 38
  mapping 41, 42, 44
dimensionality reduction 94
distance evaluation methods
  cosine similarity 32, 33
  dot product 30, 31
  Euclidean distance 30
document store 169
dot product 30, 31
dslim/bert-base-NER model 108
Dynamic Context Layer (DCL) 183

Dynamic Context Layer plugin
  architecture 189-191
  building 191
  ChatGPT integration 198
  data elevation, with Embedchain 196, 198
  deployment 204-207
  information fetching, from Elastic documentation 191-196
Dynamic Contextual Interface (DCI) 190

## E

Eland 57, 58
Elastic
  search experience 21
Elastic Cloud 39-41
  reference link 39, 59
Elastic Learned Sparse EncodeR (ELSER) 147, 153, 167
  for RAG-enhanced search 167
  for semantic search 169
  hardware consideration 154
  index, setting up 156
  limitations 162
  model, downloading in Elastic 154-156
  role, in GenAI 148
  semantic search 160-162
Elasticsearch 11, 19
  authentication, configuring 59
  image vectors, indexing 97-99
  model, deploying 61
  model, loading 61
  model, starting 61
  NLP model, deploying into 78, 79
  Sentence Transformer, loading from Hugging Face 59
URL 192
vector, generating for query 62
vectors, generating 62-65
Elastic web crawler 191
advantages 191
ELSER, integrating with RRF 171
BM25 sub-search 171
ELSER sub-search 171
RRF fusion 171
ELSER sub-search 171
email phishing detection 142
challenges 145
email spoofing 142
Embedchain
URL 196
embedding (vectors) 4
encryption 105
Enron email dataset 149-153
Euclidean distance 30, 95
existing model, logs
training or fine-tuning 126, 127

## H

Hierarchical Navigable Small World (HNSW) 11, 28-30, 94
directions and magnitude 33-36
distance metric 30
features 30
hot_threads 65
Hugging Face 8, 9, 52
datasets 55, 56
features 9
model, downloading 60, 61
Model Hub 52-54
Sentence Transformer, loading into Elasticsearch 59
spaces 56, 57
Hugging Face Hub
model, loading from 60
human-understandable descriptions
generating 128, 129

## F

FACC BEC attack 145
Facebook and Google scam 144
feature extraction 94
feature vectors 95

## G

generative artificial intelligence (GenAI) 148
ELSER role 148
generator 170
get trained models statistics API
reference link 69
grok patterns 108

## I

image search
challenges and limitations 100
evolution 93, 94
implementing 95
mechanism 94
overview 93
vector similarity search, role 94
image vectorization 96
index mapping 156, 157
information retrieval, improving in Elastic Stack
reference link 171
ingestion pipeline 157, 158
inverse document frequency (IDF) 23

## K

k-Nearest Neighbor (kNN) search 6, 44, 94
  mapping 45
  using, example 45-48
key considerations, RAG
document store 169
generator 170
retriever 169

## L

language models, and RAG 172
expansion of LLMs 172
pre-trained LLMs 172
semantically indexed data,
  feeding to LLMs 172
large language models (LLMs) 17, 148, 184
lexical search 168
Llama 2
  URL 172
load testing 80
  Rally 80
  RAM estimation 84
  slowdown, troubleshooting 87
log analytics
  embeddings, applying 126
  observability, role 125, 126
  vectors, applying 126
log vectorization 129
  Apache HTTP server 130
  AWS CloudTrail 131
  Elasticsearch 133
  Kafka (server log) 133
  Linux audit log 133
  Microsoft Windows event log 130
  MongoDB (log format) 133
  MySQL (general query log) 133
  Nginx (combined log format) 130
  Syslog (RFC 5424) 131
  synthetic log, generating 130
  used, for building query 135, 136

## M

machine learning
  pipeline 4
marketplace, vector search
  Elasticsearch 11
  Milvus 10
  Pinecone 10
  Vespa.ai 10
  Weaviate 11
masking 105
mean time to resolve (MTTR) 124
Milvus 10
ML node capacity
  planning 68, 69
Model Card 53
modern software systems
  observability 124
multi-modal models
  vector space 101
multi-modal models, for vector search 100
  integrative approach 100

## N

Named Entity Recognition (NER) models 12, 14, 108
  PII redaction, performing with 108
Natural Language Processing (NLP) 108
  techniques, augmenting 147
natural language Python toolkit
  URL 15

## NLP model

deploying 77, 78
deployment configurations 79, 80
loading, into Elasticsearch 78
NotPetya malware attack 144
types 143
vishing 143
whaling 143
phreaking 142
PII redaction
  NER models and regex (or grok)
    patterns, combining for 109
  with NER models 108
  with regex patterns 108, 109
PII redaction pipeline, in Elasticsearch 110
  cloning, to create different versions
    for different data streams 118
  contextual awareness,
    implementing 119-121
  customizing options 118
  default PII example, customizing 118
  default pipeline, installing 113-117
  expanding options 118
  expected results 117
  NER models, fine-tuning for
    particular datasets 119
  synthetic PII, generating 110-113

## O

observability 20, 124
  logs 124
  metrics 125
  pillars 124, 125
  role, of log analytics 125, 126
  traces 125
OpenAI
  logs at write, expanding 133-135
OpenAI clip-ViT-B-32-multilingual-v1
  model 101, 102

## P

pandas API 57
Personal Identifiable Information (PII) 105
  leaking 107
pharming 143
phishing 142
  reference link 142
phishing attacks
  Business Email Compromise (BEC) 143
  clone phishing 143
  examples 144, 145
  frequency statistics 144
  pharming 143
  smishing 143
  spear phishing 143
PII storing, in logs
  risks 106
PII types
  identifying, with regex/grok patterns 109
Pinecone 10
pre-trained LLMs 172
Principal Component Analysis (PCA) 70
pseudonymization 105

## Q

quantization 73, 74
Question-Answering (QA) 12, 17, 18

## R

RAG-enhanced CookBot implementation 172
bulk indexing operation, performing 178
bulk indexing sequence, creating 177
connection, establishing with Elasticsearch instance 174
CSV file, reading 176
data, bulk indexing 176
data frame, converting into list of dictionaries 176
data, preparing 174
generator, building 180
generator, integrating into Streamlit application 181, 182
index, creating 176
index, defining 175
mapping, creating for index 175
preprocessing pipeline, defining for documents 176
retriever, building 178, 179
retriever, leveraging 180
RAG-enhanced search with ELSER and RRF 167
Rally 80, 81
challenge 81
configuring 82
Python environment, setting up 82
race 81
race, running on existing cluster 83
track 81
RAM estimation 84
results, evaluating 85, 86
vector search, benchmarking 84, 85
Real-Time Contextual Fetcher (RCF) 191

reciprocal rank fusion (RRF) 167
for RAG-enhanced search 167
recurrent neural networks (RNNs) 13
redaction 105
regular expressions(regex) patterns 108
PII redaction, performing with 108, 109
relevancy model 22
BM25 24-26
TF-IDF 23, 24
retrieval augmented generation (RAG) 167, 168
essential considerations 169-171
retriever 169
RRF fusion 171

## S

Sandworm 145
search experience
evolution 26
keyword-based search, limits 26, 27
vector representation 27
Semantic Embedding Generator (SEG) 190
semantic search
with ELSER 169
semantic search, on logs 135-140
ingest pipeline 137, 138
model, loading 136
query, building with log vectorization 135, 136
Sentence Transformer
loading, from Hugging Face into Elasticsearch 59
sentiment analysis 14, 15
Significance of Context (SoC) 186
  example 188, 189
  factors 186
  k in sigmoid function 187
site reliability engineering (SRE) 140
smishing 143
sparse vectors 36, 38
spear phishing 143
SQuAD 18
StackOverflow vector track 81
storage efficiency strategies 70
  dense_vector from _source, excluding 75, 76
  dimensionality, reducing 70-72
  quantization 73, 74
supervised learning 4

## T

t-Distributed Stochastic Neighbor Embedding (t-SNE) 70
term frequency-inverse document frequency (TF-IDF) 23, 24
term frequency (TF) 23
text classification 15, 16
text summarization 18, 19
Transformers library 55
troubleshooting slowdown 87
  Hot Threads API 87
  indexing considerations 89
  page faults 88, 89

## U

Ubiquity Networks social engineering attack 145
Ukrainian power grid attack 145
unsupervised learning 4

## V

vector data type 36
vectorization 94
vector representation 27
  HNSW 28, 29
  vectorization process 28
vector search 5, 168
  AI-based search 12
  application domains 11
  applications 5, 6
  cybersecurity 20
  developer experience 6, 8
  implementing, for diverse media types 102
  k-nearest neighbors (kNN) search 6
  marketplace 10
  observability 20
  use cases 11
Vector Search Engine (VSE) 190
vector search implementation, for diverse media types
  ecommerce platforms 103
  new agencies 102
  social media platforms 103
vector search, with images 96
  image vectorization 96, 97
  k-Nearest Neighbor (kNN) search 99, 100
Vespa.ai 10
Vision Transformers (ViT) 94

## W

Weaviate 11
whaling 143
word embeddings 28
