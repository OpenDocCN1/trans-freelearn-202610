![](bbox=[0.11, 0.1, 0.86, 0.34])

Figure 2.8 – Network architecture for private pools

With this networking functionality, not only can you access resources within your VPC, but also source code management systems and artifact registries that may reside on-premises connected to your VPC by a private connection such as a Virtual Private Network (VPN) tunnel or a Direct Interconnect.

In addition to configurable networking, private pools allow users to specify a specific Google Cloud region in which their private pool and workers will reside. This is in contrast to the default pool, which executes on a worker that can run in multiple regions around the globe, depending on resource availability.

Private pools can be created in the following regions, at the time of publication:

+   - Asia:
+     - asia-east1 (Taiwan)
+     - asia-east2 (Hong Kong)
+     - asia-northeast1 (Tokyo)
+     - asia-northeast2 (Osaka)
+     - asia-northeast3 (Seoul)
+     - asia-south1 (Mumbai)
+     - asia-south2 (Delhi)
+     - asia-southeast1 (Singapore)
+     - asia-southeast2 (Jakarta)