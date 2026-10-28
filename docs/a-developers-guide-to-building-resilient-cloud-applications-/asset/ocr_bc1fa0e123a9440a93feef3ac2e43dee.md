# Working with Azure Storage

Azure Storage is a Microsoft cloud storage solution for more than just data storage. Azure Storage offers a scalable and secure object store for a variety of data objects, a filesystem service in the cloud, a messaging store for relevant messaging, and an SQL store.

This chapter will cover the different storage options that are available in Azure Storage and the scenarios in which each storage option is appropriate.

In this chapter, we’re going to cover the following main topics:

+   - Azure Storage account
+   - Exploring Azure Table storage
+   - Exploring Azure Blob Storage
+   - Exploring Azure Disk Storage
+   - Exploring Azure Files

# Azure Storage account

In the healthcare solution, we have a platform for patient medical records. It includes unstructured data such as documents, messages, notes, and images related to a patient, for example, diagnoses documents, annual exams, and disease surveillance. All these documents are stored on a virtual machine using a custom solution to extract or load the unstructured data.

Azure Storage is a service that can be used to store files, messages, tables, and other types of data. It’s scalable, secure, and easy to manage and can be accessed from anywhere. Azure Storage is following two cloud models: infrastructure as a service by using virtual machines and platform as a service by using native cloud services. Azure Storage includes four data services, which can be accessed through a storage account.

Each type supports different features and has a unique URL address to access. The data and your Azure Storage account are always replicated to ensure durability and high availability.