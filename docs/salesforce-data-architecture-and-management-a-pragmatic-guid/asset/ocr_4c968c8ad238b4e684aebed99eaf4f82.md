There is no need to always do a point-to-point integration involving a middleware with Salesforce: that's where Salesforce Connect and external objects come into play. We will review external objects, their limitations, and the benefits of using external objects. And when you are working with very large volumes of data going into hundreds of millions of rows or billions of rows of data, big objects come to the rescue. We will discuss the benefits of big objects and the considerations for them before wrapping up this chapter.

The topics covered in this chapter include the following:

+   - Revisiting databases
+   - **Large Data Volumes (LDVs)**
+   - Salesforce Connect and external objects
+   - Introducing big objects

By the end of our discussion, we will understand the options available to us when it comes to big data and also which option to use given the use cases we are working with.

# Revisiting databases

Before we dive deep into LDVs and some associated topics, let’s take a brief history trip. Understanding relational and non-relational databases will help us understand the different types of solutions that are available with Salesforce and assist us in determining which one to use depending on requirements. In the early days of modern computing, tapes were used for storage, and the first digital-tape storage system, IBM’s Model 726, could store 1.1 **MB** (megabytes) on a single reel of data. For comparison purposes, these days, a tape cartridge can hold up to 15 **terabytes** (**TB**). Model 726 was a non-relational database in that it was free-form records linked together.

Then came relational databases, which offered a more organized approach to store, retrieve, and search data. Data was stored in multiple tables in keeping with normalization principles and logically linked together using unique identifiers called **primary** and **foreign** keys. Refer to *Chapter 2, Understanding Salesforce Objects and Data Modeling*, for a more in-depth discussion on normalization with the help of examples.