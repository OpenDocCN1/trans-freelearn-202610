
## Chapter 14: Logging and Auditing 503

Technical requirements ......................................................... 503

Introduction to logging ......................................................... 504

- Where to log • 505
- When to log • 508
- What to log • 512

Extracting information from logs – pgBadger ......................................................... 514

- Installing pgBadger • 514
- Configuring PostgreSQL logging for pgBadger usage • 515
- Using pgBadger • 516
- Scheduling pgBadger • 521

Implementing auditing ......................................................... 524

- Installing PgAudit • 525
- Configuring PostgreSQL to exploit PgAudit • 526
- Configuring PgAudit • 527
- Auditing by session • 528
- Auditing by role • 530

Summary ......................................................... 532

Verify your knowledge ......................................................... 532

References ......................................................... 533

## Chapter 15: Backup and Restore 535

Technical requirements ......................................................... 536

Introducing types of backups and restores ......................................................... 536

Exploring logical backups ......................................................... 537

- Dumping a single database • 539
- Restoring a single database • 543
- Limiting the amount of data to backup • 547
- Compression • 548
- Dump formats and pg_restore • 549

Performing a selective restore • 552

Dumping a whole cluster • 555

Parallel backups • 556

Backup automation • 558

The COPY command • 559

### Exploring physical backups ........................................................ 563

Performing a manual physical backup • 564

pg_verifybackup • 566

Starting the cloned cluster • 567

Restoring from a physical backup • 568

### Basic concepts behind PITR ........................................................ 569

### Summary ........................................................ 570

### Verify your knowledge ........................................................ 570

### References ........................................................ 571

## Chapter 16: Configuration and Monitoring 573

### Technical requirements ........................................................ 574

### Cluster configuration ........................................................ 574

Inspecting all the configuration parameters • 576

Finding configuration errors • 578

Nesting configuration files • 579

Configuration contexts • 580

Main configuration settings • 581

WAL settings • 582

Memory-related settings • 584

Process information settings • 585

Networking-related settings • 585

Archive and replication settings • 586

Vacuum and autovacuum-related settings • 587

Optimizer settings • 587

Statistics collector • 587

Modifying the configuration from a live system • 588

Configuration generators • 589

### Monitoring the cluster ......................................................... 592

Information about running queries and sessions • 593

Inspecting locks • 594

Inspecting databases • 596

Inspecting tables and indexes • 597

More statistics • 599

### Advanced statistics with pg_stat_statements ......................................................... 600

Installing the pg_stat_statements extension • 600

Using pg_stat_statements • 601

Resetting data collected from pg_stat_statements • 602

Tuning pg_stat_statements • 602

### Summary ......................................................... 603

### Verify your knowledge ......................................................... 603

### References ......................................................... 604

## Chapter 17: Physical Replication 607

### Technical requirements ......................................................... 608

### Exploring basic replication concepts ......................................................... 609

Physical replication and WALs • 609

The wal_level directive • 610

Preparing the environment setup for streaming replication • 610

### Managing streaming replication ......................................................... 612

Basic concepts of streaming replication • 612

Asynchronous replication environment • 614

The wal_keep_segments option • 615

The slot way • 616

The pg_basebackup command • 616

Asynchronous replication • 617

Replica monitoring • 619

Synchronous replication • 620

PostgreSQL settings • 621

Cascading replication • 623

Delayed replication • 626

Promoting a replica server to a primary • 626

Summary ........................................................................ 627

Verify your knowledge ........................................................................ 628

References ........................................................................ 628

## Chapter 18: Logical Replication 631

Technical requirements ........................................................................ 631

Understanding the basic concepts of logical replication ........................................................................ 632

Comparing logical replication and physical replication ........................................................................ 635

Exploring a logical replication setup and new logical replication features on PostgreSQL 16 . 636

Logical replication environment settings • 636

The replica role • 637

Primary server – postgresql.conf • 637

Replica server – postgresql.conf • 638

The pg_hba.conf file • 639

Logical replication setup • 639

Monitoring logical replication • 641

Read-only versus write-allowed • 643

DDL commands • 649

Disabling logical replication • 651

Making a logical replication using a physical replication instance • 652

Summary ........................................................................ 657

Verify your knowledge ........................................................................ 658

References ........................................................................ 658

## Chapter 19: Useful Tools and Extensions 661

Technical requirements ........................................................ 662

Exploring the pg_trgm extension ........................................................ 662

Using foreign data wrappers and the postgres_fdw extension ................................... 665

Disaster recovery with pgbackrest .......................................................... 667

Basic concepts • 668

Environment set up • 669

The exchange of public keys • 669

Installing pgbackrest • 671

Configuring pgbackrest • 672

The repository configuration • 672

Using pgbackrest with object store support • 675

The PostgreSQL server configuration • 675

The postgresql.conf file ............................................................... 675

The pgbackrest.conf file ............................................................... 676

Creating and managing continuous backups • 677

Creating the stanza • 677

Checking the stanza • 677

Managing base backups • 678

Managing PITR • 681

Migrating from MySQL/MariaDB to PostgreSQL using pgloader ................................... 684

Summary ....................................................................... 688

Verify your knowledge .............................................................. 688

References ..................................................................... 689

### Other Books You May Enjoy 691

### Index 697

## Who this book is for

This book is for anyone interested in learning about the PostgreSQL database from scratch or anyone looking to build robust, scalable, and highly available database applications. All the newest and coolest features of PostgreSQL will be presented, along with all the concepts a database administrator or an application developer needs to get the best out of a PostgreSQL instance. Although prior knowledge of PostgreSQL is not required, familiarity with databases and the SQL language is expected.

## What this book covers

*Chapter 1, Introduction to PostgreSQL,* explains what the PostgreSQL database is, the community and development behind this great and robust enterprise-level relational database, and how to get help and recognize different PostgreSQL versions and dependencies. You will also learn how to get and install PostgreSQL either through binary packages or by compiling it from sources. You will see how to manage the cluster with your operating system tools (systemd and rc scripts).

*Chapter 2, Getting to Know Your Cluster,* shows you the anatomy of a PostgreSQL cluster by specifying what is on the file system, where the main configuration files are, and how they are used. The psql command-line utility is described in order to help you connect to the database cluster and interact with it.

*Chapter 3, Managing Users and Connections,* provides a complete description of how users and connections are managed by a running instance and how you can prevent or limit users from connecting. The concept of the “role” is described, and you will learn how to create single-user accounts, as well as groups of related users.

*Chapter 4, Basic Statements,* shows how to create and destroy main database objects, such as databases, tables, and schemas. The chapter also covers basic statements, such as SELECT, INSERT, UPDATE, and DELETE. This chapter shows how to manage the public schema on PostgreSQL 16.

*Chapter 5, Advanced Statements,* introduces the advanced statements PostgreSQL provides, such as common table expressions, MERGE, UPSERTs, and queries with RETURNING rows. This chapter will provide practical examples of when and how to use them.
