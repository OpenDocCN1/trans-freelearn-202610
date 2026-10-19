
## Contributors

### About the author

**Paul Browne** is a product and program manager currently working for Europe’s largest VC by deal flow. Most recently, he led a team from three partner institutions that won an EFMD award for ecosystem development, training over 100 eurozone companies to implement digital solutions together. This continues Paul’s previous work leading business and technical projects in the public, financial, and life science sectors across Ireland, the UK, the US, the Netherlands, Belgium, and New Zealand.

Paul’s qualifications include a degree in European business and French from Ulster University and more recent certifications in finance (ACCA), procurement (CIPS), and business strategy (ESMT). He has technical certifications from Microsoft, Oracle, and IBM and holds a master’s in advanced software engineering from University College Dublin.

Paul is also the author of *JBoss Drools Business Rules*, by Packt Publishing, and has lectured in Excel and Visual Basic at third level.

*I would like to thank the Drools, KIE, and Kogito communities, and the teams at Red Hat, IBM, and Microsoft for delivering great AI and business automation solutions, and for their ongoing support in writing this book.*

### About the reviewers

**Karina Varela** has worked with enterprise technologies for over 10 years, focusing on cloud solutions, Java technologies, and the open source culture.

Her solid knowledge is built on practical experience in development, architecture, delivery, and troubleshooting production applications and products. Having worked at Red Hat, the biggest open source company in the industry, not only does she bring a vast understanding of the open source model and the open communities but she is also largely involved in the delivery of architecture and the delivery of mission-critical software in multiple sectors around the globe. She has also been awarded the Red Hat champion recognition.

Karina is well recognized within the Java community for articles and blogs, books, talks at international tech conferences, open source contributions, and active nurturing and management of communities such as SouJava.

**Filippe Spolti** is a software engineer and an open source and cloud enthusiast. On a day-to-day basis, he works with Kogito Engine for Serverless Workflows. He worked on OpenShift V3 in its early stages, helping to bring middleware products to the cloud. He was also a technical support engineer who helped customers to solve issues and contributed to several open source projects, more recently with CNCF Serverless. He contributes to local conferences in Brazil as a speaker and track coordinator. He holds a bachelor’s degree in information systems and specializations in software engineering and information security and wrote the book *WildFly: New Feature*, and has helped other authors to be successful with their books.

> *Sharing knowledge is rewarding and it also requires dedication and a little time away from family and friends. I am thankful for the support they have given me in recent years.*

**Nishant Ravle** is an IT professional with more than 16 years of IT experience in BPM, SOA, and middleware technologies. He has extensively worked on Oracle and IBM product stacks. Currently, he is in the field of software consultancy and corporate and online training.

Nishant’s technological expertise spans primary skills and expertise in business process management and the service-oriented architecture domain, developing BPM processes using IBM BPM, Oracle BPM, jBPM, Camunda BPM, and Activiti BPM in on-site assignments in the UK, Norway, Oman, Malaysia, South Africa, Dubai, Kenya, and Qatar for requirement gathering, development, consulting, architecting, and training.

**Niall Browne** has worked extensively with Excel and web technologies. He studied maths and psychology at Dublin City University and has a keen interest in coaching and sports analytics. Most recently, he was a member of the team at The Mill in Drogheda, which co-created a financial payments project with PFS.

## Table of Contents

Preface ........................................................................ xvii

## Part 1: The Problem with Excel, and Why Rule-Based AI Can Be the Solution

1

### Wrestling with Excel? You Are Not Alone ........................................ 3

| Contents | Page |
|---|---:|
| Why do you use Excel? | 3 |
| ├─ Excel as the engine room of the business | 4 |
| ├─ Day-to-day Excel problems | 4 |
| ├─ One solution – separating information and processing instructions | 5 |
| Enterprise solutions | 6 |
| ├─ Examples of enterprise solutions | 7 |
| ├─ The chasm between Excel and enterprise solutions | 8 |
| What’s your real business problem? | 10 |
| └─ What these business problems look like in real life | 10 |
| Artificial intelligence and which type can help you best | 11 |
| ├─ What is artificial intelligence? | 12 |
| Practical artificial intelligence and business rules | 16 |
| ├─ The when … then format | 16 |
| ├─ Triage – life or death business rules | 17 |
| ├─ Business rules in your organization | 18 |
| ├─ A business rule engine you already own | 19 |
| ├─ More powerful business rule engines | 21 |
| Splitting Excel into different pieces | 22 |
| ├─ Other solutions are available | 23 |
| Summary | 24 |

2

### Choosing an AI and Business Rules Engine – Why Drools and KIE? ............ 25

| Contents | Page |
|---|---:|
| Are you reading this book for personal or business reasons? | 26 |
| Which business rule engine to use | 26 |
| What’s in a name – KIE, Kogito, or Drools? | 27 |
| Why choose KIE and Drools as our Rule Engine? | 28 |
| Open source and the Red Hat and IBM business model | 29 |
| How KIE being open source helps you | 29 |
| Open can be more secure | 31 |
| Asking for help in Drools (or in any open source project) | 31 |
| The best way to ask for help in the open source world | 33 |
| KIE, Kogito, and Drools resources | 35 |
| How to design and build enterprise solutions | 37 |
| Working with a team | 37 |
| What software architects do | 38 |
| Containers supporting Kogito, KIE, and Drools | 40 |
| Four different ways to work with business rules | 42 |
| Summary | 43 |

3

### Your First Business Rule with the Online KIE Sandbox — 45

- **Writing our first business rule** — 46
- **KIE Sandbox Extended Services** — 52
- Downloading and running KIE Extended Services — 53
- Troubleshooting KIE Sandbox Extended Services — 55
- **Running our first rule** — 56
- What is happening behind the scenes – KIE Sandbox and Services — 56
- **More on Decision Models** — 57
- Decision Models aren’t workflows — 58
- Differences between Decision Models and business rules — 58
- **A tour of the UI** — 59
- What is a variable? — 60
- What is a type? — 60
- How to fix these errors — 61
- Giving our Decision Model a name — 62
- Previous autosaved decision models — 63
- Uploading Decision Models from other sources — 63
- Option 1 – importing the sample — 64
- Option 2 – step-by-step instructions — 65
- **Other UI elements in KIE Sandbox** — 67
- Note – KIE Sandbox is only a sandbox — 69
- **Summary** — 69

## Part 2: Writing Business Rules and Decision Models – with Real-Life Examples

4

| More Decision Models, Business Rules, and Decision Tables | 73 |
|---|---:|
| **Make your decision service easy to work with** | **74** |
| What shape is your data – tables or trees? | 74 |
| Modeling our data in the KIE Sandbox | 76 |
| Using our complex data type in a decision model | 79 |
| Describing our data better using constraints | 80 |
| What makes a good custom data type? | 82 |
| **A more powerful Decision node – Decision Tables** | **83** |
| Our first Decision table in the KIE Sandbox | 83 |
| Decision Tables as your go-to solution (other nodes are available) | 87 |
| **More nodes that you can use** | **87** |
| **Other types of Decision Nodes** | **89** |
| Running your function | 93 |
| **A more sophisticated Decision Table** | **95** |
| **Rule matching and HIT Policies** | **96** |
| **A safer Decision Table example** | **97** |
| **Summary** | **100** |

5

| Sharing and Deploying Decision Models Using OpenShift and GitHub | 101 |
|---|---:|
| **How deploying to the cloud makes things easier** | **101** |
| Reality check – why not connect Excel to the local KIE Sandbox? | 102 |
| **OpenShift – Red Hat’s piece of the cloud** | **102** |
| Signing up for the OpenShift Developer Sandbox | 103 |
| Accessing your OpenShift Developer Sandbox | 105 |
| Linking KIE Sandbox and your OpenShift instance | 106 |
| Deploying your decision model to OpenShift | 109 |
| Important notes on the OpenShift Developer Sandbox | 112 |
| **Taking care of your data** | **113** |
| **Saving and sharing your decision models using GitHub** | **114** |
| Signing up for GitHub and getting your token | 114 |
| A tour of your personal homepage on GitHub | 115 |
| Using your token to collaborate on KIE Sandbox | 118 |
| GitHub flows for saving and project collaboration | 120 |
| **Summary** | **126** |
