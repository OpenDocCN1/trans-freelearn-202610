Amazon RDS manages the access and decryption of your data transparently without you having to modify the client application. The database remains unaware of the encryption and decryption operations as this is handled by the host, as depicted in the following diagram:

![](bbox=[0.137, 0.178, 0.847, 0.55])

Depending on the engine, you have options for encryption at rest. So for example, with MySQL and PostgreSQL, you would use AWS Key Management Service (KMS) and EBS encryption, which is handled by the RDS host and the underlying EBS-based storage. With KMS, you either use AWS- or customer-managed Customer Master Keys (CMKs) to generate the necessary data keys. These data keys are used to perform the actual encryption and decryption of your data.

For engines like Oracle and Microsoft SQL Server, encryption at rest is configured using Transparent Data Encryption (TDE). The difference here is that the actual encryption and decryption are handled by the database itself rather than the host.