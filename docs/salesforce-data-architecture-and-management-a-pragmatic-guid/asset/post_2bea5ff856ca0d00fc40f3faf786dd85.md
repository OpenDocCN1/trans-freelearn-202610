
This chapter should have provided you with a solid foundation to be able to understand multi-tenancy architecture, and if you come from a relational database background, some things that didn’t make sense earlier should now start to make a lot more sense. The data architectural principles learned in this chapter will help you design more scalable and efficient business applications, and understanding data modeling concepts and the different types will allow you to communicate effectively with different audiences. Understanding why having a cohesive data model that has objects linked with one another using relationships will help you adhere to the Salesforce click versus code methodology and allow you to frame conversations with different stakeholders in the right context.

In the next chapter, we will look at data management – what it is and its different facets. The knowledge acquired in this chapter will have set the stage to understand the upcoming topics with ease and clarity.

## Questions

- 1. Precision Printers has an `Applicant` object that has many rows in it and users are reporting slow performance when running reports on this object. As the resident data architect, you have been asked to investigate this and suggest a solution. What can you suggest?
- a) View the filters used in the report and tweak them to use indexes on the report.
- b) Just create an index on the database table and use that in the report.
- c) Do nothing; it’s probably just a temporary issue and will go away by itself.

- 2. Precision Printers has promoted a new integration to the production environment recently. It runs every 12 hours and transfers records from the `Account` object that have changed to an ERP system. The administrator is noticing that records are being transferred to the ERP even when there hasn’t been an apparent change made on the record. What could be causing this problem?
- a) No action is needed; the integration will correct itself and the correct records will start to sync into the ERP.
- b) Take an inventory of the records in Salesforce that should have not been synched and then manually remove these records from the ERP.
- c) The criterion for determining records that have changed is based on `SystemModStamp`. Changing the criterion to `LastModifiedDate` will fix the issue.
- d) Delete all the records from the ERP and restart the integration.

- 3. A rookie admin has created multiple objects, including two objects for storing account and contact records for a custom app that is used to track incoming requests for quotes from potentially strategic prospects. The admin has been asked to create reports to show the prospects with the largest quotes, the products they are interested in, and the number of days the quote request has been in each stage. In addition, managers want to see on the Account object whether an open request exists for a quote. The admin is being told by the developer that a lot of code must be written to produce these reports. What could be the underlying cause of writing so much code for these reports? (Choose 2 answers)
- a) Appropriate relationships have not been defined between objects and that is causing Apex code to be written to query the data.
- b) The admin should open a ticket with Salesforce Support and get them to create these reports.
- c) The data model needs to be redesigned following architectural best practices and the application redone.
- d) Create a lookup relationship between the custom Account object and the quote request object and then write a trigger to aggregate the open quote requests and display them on the custom Account object.

## Further reading

- Developer Force, Multitenant Architecture: http://www.developerforce.com/media/ForcedotcomBookLibrary/Force.com_Multitenancy_WP_101508.pdf
- Normalization, section 1.3.6, DATA MANAGEMENT BODY OF KNOWLEDGE (DMBOK)

## Understanding Data Management

In this chapter, we will look at the importance of managing data and the risk and challenges related to data management. We will also discuss some principles of data management, the different types of data, the data life cycle, and the relationship between data management and data governance. This is going to provide you with a foundational understanding of why it is critical to manage data and some of the challenges that you will come across when managing your data. We will look at some strategies on how to overcome those challenges by applying some commonly used principles.

We will also review best practices related to data management, where we will discuss strategies related to managing data and metadata. Learning these best practices will allow you to use these in your organization to implement tools and processes that will enable optimal data management and efficient use of resources. We will also review backup and restore strategies, why they are critical to business, and how to go about implementing the tools that will help you manage your data. Given that Salesforce stopped providing data restore services in 2020, this section aims to equip you with the necessary knowledge to effectively implement backup and restore procedures right out of the gate. At the end, we will discuss some popular Salesforce tools and third-party tools that can be used to manage data, including backing up and restoring it.

The topics covered in this chapter include the following:

- What is data?
- What is data management?
- Aspects of data management
- Data backup and recovery
- Tools of the trade

### What is data?

I have come across many projects where the terms *data* and *information* were used interchangeably, and this was fine in most cases, but in the same way that we established a common understanding of the term *architecture*, it would be prudent to do the same here.

Data is a representation of facts that are either stored digitally or on other media, such as physical photos or paper. It is important to acknowledge that not everything presented as data is factual, with *factual* here meaning that because it's the truth when seen in combination with other data elements, it will convey the full picture of the truth. Data shouldn’t be reviewed without considering the context of the data, because data without context causes confusion and opens the door to misinterpretation. Consider how many ways there are in which dates can be presented. In fact, it is a common source of confusion when you see a date written like 12-09-2020. Are the first two characters representing the month or the day? When you have the context, perhaps on the paper form you are filling out, or the web form that instructs you to write the date in the MM-DD-YYYY format, that's when you know that the first two characters are representing the month. Therefore, presenting data in context is important, otherwise, it loses its value and effectiveness.

Data is sometimes referred to as the raw material for information and information as data within context. There is quite a bit of discussion ongoing regarding this that is beyond the scope of this book, but for our purposes, we will stick with the definition of data as mentioned earlier. In the next section, we will ask the question of whether data is really valuable and whether we should devote so much energy to managing it properly.

### Is data valuable?

Data is and should be considered an organizational asset. This is now easy to understand given how social media users have had their data aggregated and sold to marketers in the past. Every organization, for-profit and not-for-profit alike, realizes the importance of data and tries to harvest it to gain valuable insights that can drive decision-making. For example, until a decade or so ago, very few mining companies had the knowledge or the expertise to understand fuel consumption and usage of their heavy-duty machinery, but today that data is sent through onboard sensors to centralized databases where it is aggregated and analyzed for fuel consumption. This helps the company to understand which machine is idling excessively versus those that may be using too much fuel, which could be an indication of an imminent breakdown. Proactively servicing the machine could save the company hundreds of thousands of dollars in downtime and maintenance costs.

Similarly, for a non-profit organization, understanding its donor base, their interests, and demographics are all vital for the organization’s continued operations. Using data in its system, it can analyze the data to determine which channels to focus their ads on if they are targeting a specific demographic. Similarly, data can also be used to determine which areas need the most help traditionally so the organization can focus its energy on those areas.

As the science of AI matures and gains traction, data will continue to gain importance and form the backbone of many new businesses and existing businesses will continue to look for ways to adopt it. AI is the science of training systems that can simulate tasks based on data. These systems continue to self-learn and refine their decision-making capabilities as new data becomes available. For example, Tesla uses internal and external sensors on its cars that send data back to its data centers and can then be used to show the average increase in speed over certain patches of road, determining hazardous locations that are causing drivers to take sudden action. Tesla accords data collection and analysis a high level of importance, going as far as parsing its online forum for insights.

Now that we understand the value of data, we will look at what data management is and why it's important in the next section.
