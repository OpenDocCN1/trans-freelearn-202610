
## Natural Language Processing with Flair

A practical guide to understanding and solving NLP problems with Flair

![](img/c56c48b8fd18a11ab705352d618ed976_0_0.png)

## Natural Language Processing with Flair

A practical guide to understanding and solving NLP problems with Flair

Tadej Magajna

![](img/c56c48b8fd18a11ab705352d618ed976_1_0.png)

## Natural Language Processing with Flair

Copyright © 2022 Packt Publishing

*All rights reserved.* No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Publishing Product Manager:** Ali Abidi
**Senior Editor:** Nathanya Dias
**Content Development Editor:** Nazia Shaikh
**Technical Editor:** Devanshi Ayare
**Copy Editor:** Safis Editing
**Project Coordinator:** Aparna Ravikumar Nair
**Proofreader:** Safis Editing
**Indexer:** Sejal Dsila
**Production Designer:** Aparna Bhagat
**Marketing Coordinators:** Abeer Dawe, Shifa Ansari

First published: April 2022

Production reference: 1210422

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-80107-231-1

www.packt.com

To my girlfriend and my family, who supported me throughout the entire process of writing this book. Special thanks to Prof. Dr. Alan Akbik, the original author of Flair, for help and for making the framework available to the public.

- Tadej Magajna

## Contributors

## About the author

Tadej Magajna is a former lead machine learning engineer, former data scientist, and now a software engineer at Microsoft. He currently works in a team responsible for language model training and building language packs for keyboards such as Microsoft SwiftKey. He has a master's degree in computer science. He started his career as a 15-year-old at a local media company as a web developer and progressed toward more complex problems. He has tackled problems such as NLP market research, public transport bus and train capacity forecasting, and finally, language model training in his current role. Currently, he is based in his hometown of Ljubljana, Slovenia.

## About the reviewers

After studying engineering and physics, **Pascal Tartarin** held various senior management positions (sales, marketing, and R&D) mainly in the chemical and medical device industries in Europe and Asia Pacific. A keen lifelong learner, he later developed his expertise in data science and NLP. Over the last 6 years, he has built applications in the field of business (forecasts, CRM) based on ML, deep learning, NLP, and more recently, knowledge graphs (Neo4j).

**Amardeep Kumar** has sound experience in Natural Language Processing and machine learning systems. He has industry experience of 2 years working on different technical stacks in NLP and software development. Amardeep regularly contributes to open source software development as a developer, mentor, and reviewer in Google Summer of Code programs. He has published research papers for top-tier NLP-proc conferences, such as ACL and EMNLP.

## Table of Contents

## Preface

## Part 1: Understanding and Solving NLP with Flair

## 1 Introduction to Flair
- Technical requirements 4
- A brief introduction to NLP 4
- Tokenization 5
- Text vectorization 5
- Named entity recognition 6
- Word-sense disambiguation 7
- Part-of-speech tagging 7
- Chunking 8
- Stemming and lemmatization 8
- Text classification 8
- Introducing Flair 9
- Setting up the development environment 10
- Creating the virtual environment 10
- Installing a published version of Flair in a virtual environment 10
- Installing directly from the GitHub repository (optional) 11
- Running code that uses Flair 12
- Summary 12

## 2 Flair Base Types
- Technical requirements 14
- Sentence and Token objects 14
- Understanding the Sentence class 15
- Understanding the Token class 15
- Tokenization in Flair 16
- Sentence and Token object helper methods 17
- Using custom tokenizers 18
- Using the TokenizerWrapper class 19
- Understanding the Corpus object 20
- Summary 22

## 3 Embeddings in Flair
- Technical requirements 24
- Understanding word embeddings 24
- Cosine similarity 25
- The "king – man ≈ queen – woman" analogy 25
- Classic word embeddings in Flair 33
- Flair embeddings 35
- Understanding the contextuality of Flair embeddings 35
- Character-level sequence modeling in Flair embeddings 36
- Pooled Flair embeddings 37
- Available Flair embeddings 38
- Stacked embeddings 38
- Document embeddings 39
- Other embeddings in Flair 40
- Summary 41

## 4 Sequence Tagging
- Technical requirements 44
- Understanding sequence tagging in Flair 44
- Named entity recognition in Flair 45
- Types of NER taggers in Flair 47
- Part-of-speech tagging in Flair 48
- Part-of-speech tagging tag sets 49
- Types of PoS taggers in Flair 51
- Other tagging tasks in Flair 52
- Sequence labeling metrics 52
- Measuring accuracy for sequence labeling tasks 52
- Measuring the F1 score for sequence labeling tasks 53
- Summary 54

## Part 2: Deep Dive into Flair – Training Custom Models

## 5 Training Sequence Labeling Models
- Technical requirements 58
- The motivation behind training custom models 58
- Understanding the hardware requirements for training models 59
- Working with parameters for training and evaluation 61
- Understanding neural model training 61
- Parameters and terminology used in Flair model training 63
- Training custom sequence labeling models 64
- Loading a tagged corpus 65
- Loading the tag dictionary 66
- Building the embedding stack 66
- Initializing the SequenceTagger object 67
- Training the model 69
- Understanding training output files 73
- Loading and using custom Flair models 73
- Knowing when to stop and try again 75
- Monitoring loss 76
- Assessing and comparing performance metrics 77
- Summary 77

## 6 Hyperparameter Optimization in Flair
- Technical requirements 80
- Understanding hyperparameter tuning 80
- Optimization methods 81
- Hyperparameter tuning in Python 83
- Hyperparameter tuning in Python hands-on 84
- Hyperparameter optimization in Flair 86
- Hyperparameter tuning in Flair hands-on 86
- Summary 89

## 7 Train Your Own Embeddings
- Technical requirements 92
- Understanding the how and why behind custom Flair embeddings 92
- Why training embeddings rarely ever means training embeddings 92
- Corpora for Flair embeddings 94
- Dictionaries for training Flair embeddings 95
- Evaluating word embeddings 95
- Training Flair embeddings on the world's smallest language 96
- Preparing the dictionary 97
- Preparing the corpus 98
- Training the language model 100
- Using custom embeddings on downstream tasks 102
- Performing intrinsic evaluation on custom Flair embeddings 103
- Summary 105

## 8 Text Classification in Flair
- Technical requirements 108
- Understanding text classification 108
- Training text classifiers 109
- Text classification in Flair 111
- Using pre-trained Flair text classification models 112
- Document embeddings in Flair 113
- Text classifiers in Flair 116
- Training a text classifier in Flair 116
- Working with text classifiers that require little to no training data 122
- Zero-shot classification with TARS 123
- Few-shot classification with TARS 125
- Summary 129

## Part 3: Real-World Applications with Flair

## 9 Deploying and Using Models in Production
- Technical requirements 134
- Technical considerations for NLP models in production 134
- Client-side versus server-side inference 135
- When to use managed services and when to self-serve server-side models 139
- Managed services for Flair models 143
- A model deployment decision-making flowchart 143
- Self-serving flair models 144
- A proof-of-concept self-serve tool for Flair 146
- Using managed services for deploying and using Flair models 150
- The Hugging Face Models Hub 151
- Summary 158
