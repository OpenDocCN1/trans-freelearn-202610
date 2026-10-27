
## D

- documentation-related features
  - dbt-generated documentation 416-419
  - exposures 414, 422-424
  - generated documentation 414
  - lineage graph 414-416
  - Markdown documentation 415, 425
  - source freshness report 414, 419-421
- duplicates 118

## E

- Ellie
  - URL 114
- Enterprise Resource Planning (ERP) 155
- entity 107, 114
- Entity-Relationship (E-R)
  - model 105, 107, 114
  - cardinality 115, 116
  - components 114, 115
  - example, at different levels of detail 122
  - generalization 122, 123
  - main notation 114, 115
  - specialization 122, 123
  - time perspective 120, 121
- evolution 460
- EXTERNAL TABLE command 496
- external tables 505
  - creating 506, 507
  - objects, setting up 508-511
- Extract, Load, and Transform (ELT) 159
- Extract, Transform, and Load (ETL) 159

## F

- facts
  - orphans, handling in 468, 469
  - reasons, for not matching with dimensions 469, 470
- fact table 138
- fan trap 133, 134
  - handling 134
- feature branching workflow 364
- FETCH clause 35
- forecasts 131, 132
- Foreign Key (FK) 117, 315, 439
- for loop statements 288
- four-eyes principle 170
- FROM clause 27, 28, 36
- FULL OUTER JOIN 42, 44

## G

- GDPR compliance 519
- generalization 122, 123
- generic tests 209, 304, 305
  - applying 306, 307
  - configurations 312, 313
  - configuring 311, 312
  - defining 308
  - from packages 307
  - out-of-the-box generic tests 306
  - pattern 308, 309
  - writing 309-311
- Git 364
  - branches 183
  - in dbt Cloud 364
- GitHub 54
  - Version Control 54
- GitHub account
  - creating 56-61
  - repository, setting up for dbt 61-63
- git Version Control System (VCS) 54
- GROUP BY clause 31, 36

## H

- Hashed Difference (HashDIFF) fields 147
- HAVING clause 32, 36
- header-detail use case 124, 125
  - implementation notes 125, 126
- header entity 124
- hierarchical relationships 126
  - examples 127, 128
- hierarchy exploration 128-131
- HIST models 446
  - advantages 446, 447
  - creating 447
- historical data 162
- history
  - in sample project 253-255
  - saving 246-248
  - saving, with dbt 248, 249
  - saving, with snapshots 249
- history patterns 518, 519
  - conflicting sources of truth, dealing with 526, 527
  - deletion, with deleted keys only 523-526
  - deletion, with full payload 523
  - history, storing with deletion list 522
  - history, storing with full load deletion 519-522
  - history, storing with multiple versions in input 527-532
  - history, storing with PII and GDPR compliance 533
  - schema evolution 536, 537
- HIST tables
  - snapshots, replacing with 463-465
  - tests, adding to 465, 466
- hooks 408, 409

## I

- identity 436
  - code 436
  - key 436
  - managing 436
  - semantics 436
  - time series semantics 437
- if statement 287
- ingestion patterns 495
  - basic setup 498
  - data, loading from files 501, 502
  - external tables 505-507
  - file access, authorizing 499, 500
  - file location, describing 498, 499
  - files, parsing 500, 501
  - landing tables 507
  - schema, interpolating for semi-structured data 503, 504
  - schema, providing to load CSV data 502, 503
  - semi-structured data 497
  - stage 498
  - structured data 497
- initial model
  - refactoring, to be human-readable 225, 226
- initial roles
  - overview 18, 19
- Inmon design style 144
- INNER JOIN 42
- in operator 286
- Integrated Development Environment (IDE) 186, 361
- interface segregation principle (ISP) 175
- intermediate transformation (TR) 242
- INTERSECT operator 38
- is operator 286

## J

- Jinja
  - comments 285
  - delimiters 285
  - expressions 285
  - literals, examples 285
  - statements 285
  - templating 3
- JOIN clause 29, 36, 39
  - example, of combining orders and customers 40, 41
- JOIN 39
- JOIN types 41
  - CROSS JOIN 42
  - FULL OUTER JOIN 42-45
  - INNER JOIN 42
  - LEFT OUTER JOIN 42, 45
  - NATURAL JOIN 43
  - queries, examples 43
  - RIGHT OUTER JOIN 42
  - visual representation 43
  - visual representation, of returned rows 44
  - visual representation, with sets 46, 47

## K

- keys 117, 437
  - Business Key (BK) 438
  - Foreign Key (FK) 117, 439
  - Natural Key (NK) 117, 438
  - operations 440
  - Primary Key (PK) 117, 438
  - Surrogate Key (SK) 117, 438
  - types 437, 438
  - uses 440
- Kimball method 138-140
- Kimball warehouse
  - downsides 140, 141

## L

- landing tables 507
  - loading, from CSV files 511-514
  - loading, from semi-structured files 514-517
  - loading pattern, using 517, 518
  - objects, setting up 508-511
- Least Surprise Principle (LSP) 175, 224
- LEFT OUTER JOIN 42, 45
- light data model
  - composite fact, defining 354
  - designing, for data mart 341-343
  - designing, for REF layer 350, 351
  - dimension enrichment 352, 353
  - master data application 354
  - REF model, defining from sources 351, 352
  - Snowflaked dimension, connecting to 353, 354
- LIMIT clause 35, 36
- lineage graph 391
- logical data model 110, 111
  - example 111, 112
- logical operators 37
  - examples 286

## M

- macros
  - basics of writing 284
  - code, producing 277
  - conditional behavior, adding 280-282
  - consistency, maintaining by reusing code 276
  - CTE, refactoring 278-280
  - data from CSV file, fixing 282-284

## P

- orphan key 47
- orphans
  - handling, in facts 468, 469
  - replacing, with default records 470-472
- parameters, save_history macro
  - analyzing 450, 451
  - diff_column parameter 451
  - high_watermark_column parameter 452
  - high_watermark_test parameter 452
  - history_filter_expr parameter 451
  - input_filter_expr parameter 451
  - input_rel parameter 451
  - key_column parameter 451
  - load_ts_column parameter 451
  - order_by_expr parameter 452
- Personally Identifiable Information (PII) 519
  - handling, with delta export 535
  - handling, with full exports 534, 535
- physical data model 110, 113
- Pivoted (PIVOT) model 243
- PIVOT model 433
- post-hook 264, 408
- Pragmatic Data Platform (PDP) 150, 151, 427
  - architecture 226
  - data mart, creating 245, 246
  - delivery layer 428
  - refined layer 428
  - refined layers, goals and contents 241, 242
  - REF model, connecting to STG 240, 241
  - Staging layer, creating 227
  - staging models, goals and content 231
  - storage layer 428
- predicate 30
- pre-hook 264, 408
- Primary Key (PK) 113, 117, 314, 438
- private packages 294
- privileges 5
  - granting, to create database 20, 21
- product increment 334
- Proof of Concept (POC) 335
- Pull Request (PR) 201, 361, 362, 390
  - creating 372-377
  - working with 363, 364

## Q

- QUALIFY clause 33-36
- Quality Assurance (QA) 183, 370
- query 23

## R

- raw data 154, 162
- REF_ABC_BANK_SECURITY_INFO model
  - renaming 466, 467
  - run_migrations macro, refactoring 467, 468
- refactoring 460
- reference architectures
  - refined data, storing 162
  - source data, storing 161
- ref function 98, 163
- refined data layer 207
- refined layer, PDP 428
  - master data mapping 431
  - master data refined layer 431
  - model types 242, 243
  - modularity 431
  - refined individual system 431
  - refined model, renaming 243-245
- REF model 242, 432
  - building, with dimensional data 271, 272
  - defining, from sources 351, 352
- Relational Databases (RDBs) 4
- relationship 107, 115
- reliability
  - providing 303, 304
- repository (repo) 55
- return() function 290
- RIGHT OUTER JOIN 42
- roles 5
  - creating 19
  - granting 19
- roles, in modern data stack
  - analytics engineer 169
  - data analyst 169
  - data engineer 169
- rows 5, 6
- run-operation command 408, 410
- run_sql() function 289

## S

- save_history macro 447
  - analyzing 448-450
  - load_from_input CTE - incremental run, analyzing 453-455
  - load_from_input CTE - non-incremental run, analyzing 455
  - parameters, analyzing 450-453
- schema 5, 501
  - issues 501
  - semi-structured data, in CSV files 501
  - structured data, in CSV files 501
  - unstructured data, in CSV files 501
- schema tests 209
- SELECT clause 25-27
- self-completing dimension 472, 487
  - position fact, restoring to keep data unchanged 491
- self_completing_dimension
  - macro, creating 487-490
- self-completing Security dimension
  - creating 490, 491
- test, moving on orphan PK of position fact 491, 492
- self-service BI 143
- Service-Level Agreement (SLA) 419
- set operators 38
  - EXCEPT 38
  - INTERSECT 38
  - MINUS 38
  - UNION [ALL] 38
- set statement 288, 289
- signature DV model
  - hub 145
  - link 145
  - satellite 145
- Single Responsibility Principle (SRP) 174, 225
- singular tests 209, 304, 305
  - creating 305
- sliding window frame 47, 50
- Slowly Changing Dimension of type 2 (SCD2) 248, 492
- snapshot 249-251
  - best practices 252, 253
  - check strategy 251
  - identity 250
  - REF layer, connecting with 255, 256
  - replacing, with HIST tables 463-465
  - running, causes 251
  - SELECT query, coding in 251
  - tables, layers 252
  - timestamp strategy 250
