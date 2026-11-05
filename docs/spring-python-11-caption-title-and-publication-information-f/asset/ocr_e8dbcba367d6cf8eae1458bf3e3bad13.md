+   Chapter 3: Adding Services to APIs 51
+   AOP from 10,000 feet 52
+     Crosscutting versus hierarchical 52
+     Crosscutting elements 53
+     Weaving crosscutting behavior 53
+   Adding caching to Spring Python objects 54
+     Applying many advisors to a service 65
+     Performance cost of AOP 68
+   AOP is a paradigm, not a library 69
+     Distinct features of Spring Python's AOP module 71
+     The risks of AOP 72
+     AOP is part of the Spring triangle 73
+   Testing our aspects 73
+     Decoupling the service from the advice 74
+     Testing our service 76
+     Confirming that our service is correctly woven into the API 78
+   Summary 79
+   Chapter 4: Easily Writing SQL Queries with Spring Python 81
+   The classic SQL issue 82
+     Parameterizing the code 84
+     Replacing multiple lines of query code with one line of Spring Python 86
+   The Spring triangle—Portable Service Abstractions 86
+   Using DatabaseTemplate to retrieve objects 87
+     Mapping queries by convention over configuration 89
+     Mapping queries into dictionaries 89
+   DatabaseTemplate and ORMs 90
+     Solutions provided by DatabaseTemplate 90
+     How DatabaseTemplate and ORMs can work together 91
+   Testing our data access layer with mocks 92
+   How much testing is enough? 94
+   Summary 95
+   Chapter 5: Adding Integrity to your Data Access with Transactions 97
+   Classic transaction issues 98
+     Creating a banking application 99
+     Transactions and their properties 101
+     Getting transactions right is hard 102
+   Simplify by using @transactional 102
+     More about TransactionTemplate 105