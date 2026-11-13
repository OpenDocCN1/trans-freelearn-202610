```
I/bob: 42
I/jim: 56
I/Ed: 22
I/Divij: 23
I/Merwyn: 24
```

Click on SEARCH, passing in the name you are searching for. Note that just the rows matching the search term are returned:

![](bbox=[0.114, 0.294, 0.725, 0.354])

Finally, type a name in EditText next to the DELETE button and then tap on the DELETE button:

![](bbox=[0.114, 0.47, 0.725, 0.537])

Now, click on SELECT and we can see that "bob" is no longer part of the database:

```
I/delete() := DELETE FROM names_and_addresses WHERE name = 'bob';
I/jim: 56
I/Ed: 22
I/Divij: 23
I/Merwyn: 24
```

Let's review what we have done in this chapter.