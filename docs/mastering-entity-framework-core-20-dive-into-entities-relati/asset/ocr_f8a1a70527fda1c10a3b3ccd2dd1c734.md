If we change the query to be based on Id, we can see that it magically uses seek instead of a scan operation. The following screenshot proves that the seeking operation is performed as expected:

![](bbox=[0.259, 0.192, 0.738, 0.651])