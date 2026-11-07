# InnoDB and PBXT

InnoDB is a MySQL storage engine having its own webpage at http://www.innod...com. It is developed by Innobase Oy, a subsidiary of Oracle. As the InnoDB sub-system must be activated by a system administrator, it may not be available on every MySQL server.

The PrimeBase XT storage engine or PBXT (http://www.primebase.org) is available for MySQL 5.1 and 6.0, and is developed by PrimeBase Technologies. The minimum MySQL required version is 5.1, as this version supports the pluggable storage engine API that is used by PBXT and other third-parties to offer alternative storage engines. This transactional storage engine is newer than InnoDB. It is usually installed after downloading it from their website and then going through a compile step. For some operating systems, a precompiled binary is available – please visit the aforementioned website for download and installation instructions.

When considering the relational aspect, here are the benefits of using the InnoDB or PBXT storage engine for a table:

+   - They support referential integrity based on foreign keys, which are the keys in a foreign (or reference) table. By contrast, using only phpMyAdmin's internal relations (discussed later) brings no automatic referential integrity verification.
+   - The exported structure for InnoDB and PBXT tables contains the defined relations. Therefore, they are easily imported back for better cross-server interoperability.

The foreign key feature of these storage engines can effectively replace the part of phpMyAdmin's infrastructure that deals with relations. We will see how phpMyAdmin interfaces with the InnoDB and PBXT foreign key system.

> > The other parts of phpMyAdmin's infrastructure (for example, bookmarks) have no equivalent in InnoDB, PBXT, or MySQL. Hence, they are still required to access the complete phpMyAdmin feature set. However, in MySQL 5.x, views are supported, and have similarities with phpMyAdmin's bookmarks.

![](bbox=[0.19, 0.64, 0.24, 0.68])

# Linked-tables infrastructure

The relational system's infrastructure is stored in tables that follow a predetermined structure. The data in these tables is generated and maintained by phpMyAdmin on the basis of our actions from the interface.