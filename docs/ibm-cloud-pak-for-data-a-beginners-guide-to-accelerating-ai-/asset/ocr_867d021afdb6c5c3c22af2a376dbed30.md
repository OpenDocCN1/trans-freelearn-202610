# 11

# Storage

**Cloud Pak for Data** leverages the Kubernetes concept of **persistent volumes**, which is supported in OpenShift, to enable its various services so that it can store data and metadata. Cloud Pak for Data services can also connect to remote databases, lakes, and object stores, as well as remotely exported filesystems as sources of data. We will look at the concepts and technologies that power both in-cluster PVs and off-cluster external storage. By the end of this chapter, you will have learned about what options are available in various public and private cloud infrastructures, as well as how to best optimize data storage for your use cases. You will also learn how storage should be operationalized, especially to support the continuous availability of your entire solution.

In this chapter, we're going to cover the following main topics:

+   - Persistent volumes
+   - Connectivity to external volumes
+   - Operational considerations