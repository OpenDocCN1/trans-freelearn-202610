
## Linking together datasets

When building each analytical dataset, you should ask yourself *how you can link them to other analytical datasets?* Which fields are natural bridges to related key datasets? Then, intentionally build the analytical dataset in a way that makes it easy to connect to others. This is typically done by creating a new or using an existing identifier key. The identifier key would be the same in both datasets.

If this sounds familiar it is the exact same concept as with relational database design. It is also closely related to the star schema design in Online Analytical Processing (OLAP) data warehousing. The goal is, however, somewhat different.

With relational database design, the goal is to minimize or even eliminate data duplication by denormalizing the data into multiple linked tables. Denormalizing separates the identifier from its description in different tables, so that the description is only stored once.

With star schema design, the goal is to make drill down, drill through, and predefined metric calculations easy and fast. Datasets are stored and linked along dimensions such as time, category, fiscal year, and company divisions.

However, with LAD design, the goal is to minimize joins while still allowing easy creation of hybrid datasets, not previously conceived. The goal is to minimize data transformation work for the IoT analyst, who is building training sets for ML modeling. The tradeoff is in data size, the initial ETL complexity when developing the analytical datasets, and the duplication of data.

- 2. **If necessary, combine multiple fields to create a single field that identifies the linkage**: Big data systems handle single field joins much better than multiple field joins. It also makes it much simpler for the data scientist to use and therefore less likely to make a mistake in combining datasets. Following our example, you can combine a slightly rounded latitude and longitude value into a single identification field and store it in a separate field in your dataset. The rounding is to adjust for extremely precise GPS values that are at the location, but just in different areas of the parking lot.
- 3. **Repeat for all linkable fields in the dataset**: The GPS grid identifier is another candidate.
- 4. **Add the dataset and its links to a master diagram to use as reference.**

The following diagram shows how our simple GPS example can link to other analytical datasets:

![](img/9f2a55db1236f995dcd9392acbc82910_322_0.png)

## Managing data lakes

Data lakes are places used to dump tons of potentially valuable data from multiple sources. Some sources will be IoT devices, while some sources will be internal company data such as production, purchasing, or customer service records. The concept is to put all of this variety of data in one place so it can be accessible through a unified interface. In the case of Hadoop, the data lake would be stored in HDFS and probably accessed through Hive or Spark.

### When data lakes turn into data swamps

Swamps are formed when water flows into an area where it collects and stagnates. Algae covers over the water. When a data lake has a mass of raw data flowing in but no organization and little usage of it to mix up the waters, it becomes what is facetiously referred to as a data swamp.

This often happens when the decision is made to copy data from many systems into one area, such as HDFS, without any changes to it. Analysts can find it difficult to access due to security restrictions. Even when they do have access it, they find it difficult to make sense of the plethora of (typically) relational tables, with no instruction key on what the ID field codes mean or what business logic should be used when working with the data.

They often give up in frustration. In such a case, a huge amount of potentially valuable data festers, unused, eating up storage space and money.

### Data refineries

Well functioning data lakes are not actually uniform pools of data water. They are more like a system of petroleum refineries. In a petroleum refinery system, the raw West Texas Intermediate crude flows in by pipeline or supertanker into several different refineries. The refineries then process it into several different refined petroleum-based products, from gasoline to petroleum jelly and even animal feed.

Sometimes, the results of one refinery are an input into another refinery, which further processes and combines it with other refined products. Not to take the analogy too far, but nobody takes the crude oil out of the ground and puts it in their gas tank.

A very similar process happens with data refineries. Nobody uses the raw crude data; they use the high-value processed and finished products. The following diagram demonstrates the concept:

![](img/9f2a55db1236f995dcd9392acbc82910_324_0.png)

Data refineries operating inside your data lake

### Developing a progression process

Data science is all about experimentation. IoT analytics has great potential. However, no one is certain of how it will develop. Put them both together and an enormous amount of experimentation should be expected. To keep this from becoming an impossible-to-manage mess, there needs to be a way to progress ad hoc datasets from early development into repeatable and stable data products.

Setting up a progression process will help manage this. Data science is highly iterative, which makes it difficult to find a clear point that signals a change in state, as with normal database development projects.

A way to handle this is by setting up regular review periods where a team determines which datasets are ready to progress to the next stage of development. Decide how often this should happen, define the stages and the corresponding requirements for each, all based on your unique situation.

We will review a suggested progression path that you can tailor to the needs of your unique IoT analytics environment.

Segment your data lake into three general areas:

- **Sandbox:** A data scientist has full access to read and write in this area. They may have a sandbox to themselves in addition to one for the team. This is for initial experimentation and model development.
- **Mature:** A data scientist has full access to this area but does not have their own mature environment, the team shares it. All code and scripts used to generate datasets kept in this area should be under source code control.
- **Production:** A data scientist has full read access but no write capability. Datasets in this area have been fully tested, code and scripts used to generate datasets are under source code control, and a change control process is in place.

The suggested progression process to move datasets between areas is here:

- **Establish a regular recurring review of your datasets:** This could be every month, every quarter, or semi-annually. Use whichever makes the most sense for how quickly your IoT analytics is developing. But the review should be regular and enforced. It is too easy to delay due to the small iterations that occur with analytics. Avoid this trap and force your team to do it.
- **Review the datasets in all three areas for any that should be deleted:** These could be development datasets that never made it far, or old versions of ones that are now in production. Take a cue from Java and have a regular garbage collection event to keep your data lake optimized.
- **Review Sandbox datasets that are ready to move to the Mature area:** These are the ones that are either project specific and ready for team testing, or are useful to the team for their general work. The latter should be set up as regularly recurring and scheduled jobs to build the datasets. This type of datasets will help accelerate many projects in the future.
- **Review Mature datasets for ones that are ready to move to Production:** These are typically more project-specific and have passed all the testing. Once it is moved into production, future changes should be minimal. At this point, control of the dataset should be handed over to a separate group to maintain and provide service-level support.

### The data retention strategy

Even big data eventually gets too big and costly to maintain. Remembering the goal of minimizing costs while still maximizing value, make sure to develop a retention strategy for IoT data. Data could be simply deleted after it is retained for a certain amount of time. However, by doing this, you could miss out on building a future profitable analytics service that was not thought of before the data was thrown away.

There are other options that allow you to retain value of the data while minimizing the costs. We will discuss some of these next.

#### Goals

The goals of a retention strategy for IoT analytics are twofold:

- **Maintain Value**: Advanced modeling techniques, such as deep learning, need lots of history to maximize prediction effectiveness. It is also difficult to know ahead of time which fields will be valuable for a future unknown project. The traditional data retention strategies of storing records for a fixed period of time and then deleting the full dataset could result in lost profitable opportunities. A different mindset is needed.
- **Minimize Costs**: IoT data can get big quickly. Even with cloud-based HDFS storage, costs can get large. Keeping everything forever could easily cost more than the value it provides. The more accessible data is, the more costly it becomes. There should be some compromises to keep your costs low.

#### Retention strategies for IoT data

We will cover three strategies to reduce the storage size of data. These can be used individually or in combination. We will discuss the pros and cons of each and walk through an example retention plan that takes advantage of all three.
