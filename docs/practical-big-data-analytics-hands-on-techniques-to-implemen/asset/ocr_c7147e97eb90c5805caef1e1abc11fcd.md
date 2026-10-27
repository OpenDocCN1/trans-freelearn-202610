# Brytlyt

**Brytlyt** is a recent entrant in the space of GPU databases. It is complemented by a visualization product called **Spotlyt**. Early testing has shown that Brytlyt surpasses several challenging benchmarks. However, how well it generalizes in other use cases remains to be seen.

Brytlyt is available in the Amazon AWS Marketplace (https://aws.amazon.com/marketplace/pp/B074JZNSWZ?qid=1513342415797sr=0-1&ref_=srh_res_product_title) for those wish to try it.

# MapD

MapD was one of the early developers of a commercial GPU database platform. Similarly to Brytlyt, it has also shown impressive early results. Nevertheless, as GPU-based databases are still in their early stages, popular use and adoption will ultimately determine whether they will become commonplace in enterprise.

One of the primary challenges of GPU-based databases is the need to configure a GPU-based system properly. This can require specialized skills, as using GPU cards for computation is quite different than using GPU cards for common tasks such as rendering images. Due to this, users wishing to try out GPU-based databases prior to adopting a formal version would find it easier to leverage a pre-configured image in AWS (AMI Image), which would require minimal system configuration.

# Other common databases

There are various other types of databases, such as ones that are used for analyzing streaming data (Amazon Kinesis), and those that process data using specialized Accelerator Cards using FPGAs from Baidu.