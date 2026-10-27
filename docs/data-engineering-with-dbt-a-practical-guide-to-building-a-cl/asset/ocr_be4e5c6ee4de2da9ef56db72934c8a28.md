The following is a list of reasons why I do not like DVs:

+   - The creation of a DV platform requires the presence of skilled data professionals with training and experience in DV2, which is a hard requirement to meet in many situations.
+   - DV models require a huge number of joins to collate all the bits and pieces of data back in the correct form. This is generally beyond the comfort zone of most developers and certainly out of reach for analysts and business users.
+   - The creation of a proper DV requires a thorough analysis of the business concepts of the domain under development. This requires sponsorship at a high enough level, some time, availability of experts on the business side, and someone who is able to put all that together.

The DV methodology has been able to adopt the best from previous design styles while adding noteworthy improvements: like Inmon, it puts business concepts at the core of the DV organization, but preserves original data; it uses the de facto standard Kimball star schemas to deliver easy-to-use data marts for the users of BI tools; it innovates by separating the ingestion and storage of source data from the application of the business rules, to decouple the two most challenging aspects of building and maintaining a data platform.

Given the common focus on business concepts, the following quote from Bill Inmon will not be a surprise:

> “The data vault is the optimal choice for modeling the Enterprise Data Warehouse in the DV 2.0 framework.”

In conclusion, we can easily say that the DV methodology is extremely attractive in all cases where the size of the development and the complexity of the requirements justify the availability of the highly skilled people required and the high-level sponsorship and visibility.

The good news is that dbt is well suited to support development with the DV methodology. To give you a quick start, allowing you to quickly apply the correct loading patterns, there is an open source dbt library named dbtvault that is available for download through the dbt hub at https://hub.getdbt.com/datavault-uk/dbtvault/latest/.

# Data mesh

The hottest buzzword at the moment in the data world is **data mesh**, so let’s have a look at it.

The data mesh approach was born in some of the most tech-heavy companies to obviate a very common problem that affects most of the data engineering industry: data ownership.

The pain is real and widespread: the people in charge of building the enterprise-wide unified data platform have no knowledge of the source data and the business; they receive at best weak support from their colleagues that run the business applications and have little to no opportunity or ability to interact with the key people that really understand the business and how data is used and interpreted on the business side.