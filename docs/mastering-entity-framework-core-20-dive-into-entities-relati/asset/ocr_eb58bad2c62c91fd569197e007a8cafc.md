The xlock used in the preceding query will ensure that no other read/write operations can be performed against the entity. Let's test the functionality by refactoring the Edit method using transactions, which also uses xlock before performing the update:

![](bbox=[0.103, 0.164, 0.896, 0.432])