3. In the next window, choose **Create storage**. Please do not confuse Cloud Shell storage with Azure Data Lake Storage. Cloud Shell uses this storage account to persist internal files. We will **not** be using this storage to store Electroniz lakehouse data:

![](bbox=[0.12, 0.16, 0.85, 0.36])

Figure 5.14 – Cloud Shell storage

4. You should now be connected to the Bash shell with a window that appears similar to the following:

![](bbox=[0.28, 0.44, 0.68, 0.58])

Figure 5.15 – The Cloud Shell screen

5. Throughout this book, we will be using the **Packt Publishing** Git repository to download code files and sample data. This is a good time to clone the Git repository so that we can access the required files. Invoke the following command inside the Bash shell:

```
git clone https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse.git
```