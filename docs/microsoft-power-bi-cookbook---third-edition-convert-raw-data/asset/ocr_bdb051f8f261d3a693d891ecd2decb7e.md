# How to create hierarchies and groups

To implement this recipe, use the following steps:

+   1. In the Table view, create the following seven calculated columns in the Accounts table:

```
Path = PATH('Accounts'[AccountKey],'Accounts'[ParentAccountKey])
Account Level 1 =
    VAR __Key = PATHITEM('Accounts'[Path],1,1)
    VAR __Result =
LOOKUPVALUE('Accounts'[AccountDescription],'Accounts'[AccountKey], __Key)
RETURN
    __Result
Account Level 2 =
    VAR __Key = PATHITEM('Accounts'[Path],2,1)
    VAR __Result =
LOOKUPVALUE('Accounts'[AccountDescription],'Accounts'[AccountKey], __Key)
RETURN
    __Result
Account Level 3 =
    VAR __Key = PATHITEM('Accounts'[Path],3,1)
    VAR __Result =
LOOKUPVALUE('Accounts'[AccountDescription],'Accounts'[AccountKey], __Key)
RETURN
    __Result
Account Level 4 =
    VAR __Key = PATHITEM('Accounts'[Path],4,1)
    VAR __Result =
LOOKUPVALUE('Accounts'[AccountDescription],'Accounts'[AccountKey], __Key)
RETURN
    __Result
Account Level 5 =
    VAR __Key = PATHITEM('Accounts'[Path],5,1)
    VAR __Result =
LOOKUPVALUE('Accounts'[AccountDescription],'Accounts'[AccountKey], __Key)
RETURN
    __Result
Account Level 6 =
    VAR __Key = PATHITEM('Accounts'[Path],6,1)
    VAR __Result =
LOOKUPVALUE('Accounts'[AccountDescription],'Accounts'[AccountKey], __Key)
RETURN
    __Result
```