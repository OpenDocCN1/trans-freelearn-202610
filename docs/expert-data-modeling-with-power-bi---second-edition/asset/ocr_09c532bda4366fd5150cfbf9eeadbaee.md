```
, PATHITEM('Parent Child'[Path], 1, INTEGER)
)
```

The following expression returns the values of the Name column for level 2 of the hierarchy:

```
Level 2 Name =
    IF(
        'Parent Child'[Path Length] >=2
        , LOOKUPVALUE(
            'Parent Child'[Name]
            , 'Parent Child'[ID]
            , PATHITEM('Parent Child'[Path], 2, INTEGER)
        ) //End LOOKUPVALUE
        , 'Parent Child'[Level 1 Name]
    ) // End IF
```

The following expression returns the values of the Name column for level 3 of the hierarchy:

```
Level 3 Name =
    IF(
        'Parent Child'[Path Length] >=3
        , LOOKUPVALUE(
            'Parent Child'[Name]
            , 'Parent Child'[ID]
            , PATHITEM('Parent Child'[Path], 3, INTEGER)
        ) //End LOOKUPVALUE
        , 'Parent Child'[Level 2 Name]
    ) // End IF
```

The following expression returns the values of the Name column for level 4 of the hierarchy:

```
Level 4 Name =
    IF(
        'Parent Child'[Path Length] >=4
        , LOOKUPVALUE(
            'Parent Child'[Name]
            , 'Parent Child'[ID]
            , PATHITEM('Parent Child'[Path], 4, INTEGER)
        ) //End LOOKUPVALUE
```