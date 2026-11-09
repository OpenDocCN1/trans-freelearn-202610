In order to decide between options 2 and 3, we will need to count how many messages will be affected using only a 1999-2002 window. We can use the following SQL:

```
SELECT count(*) FROM message
WHERE year(date) < 1998 or year(date) > 2002;
Result: 325
```

325 messages with bad dates may initially seem like a lot, but then again, they are only about 1 percent of the entire dataset. Depending on our goals, we might decide to fix these dates manually, but let's assume here that we do not mind losing 1 percent of the messages. We can proceed cautiously toward option 3, throwing out the affected e-mails. Here is the amended query:

```
SELECT date(date) AS dateSent, count(mid) AS numMsg
FROM message
WHERE year(date) BETWEEN 1998 AND 2002
GROUP BY dateSent
ORDER BY dateSent;
```

The cleaned data now consists of 1,211 rows, each with a count. Here is an excerpt of the new dataset:

| dateSent | numMsg |
|---|---|
| 1998-01-04 | 1 |
| 1998-01-05 | 1 |
| 1998-10-30 | 3 |
| 1998-11-02 | 1 |
| 1998-11-03 | 1 |
| 1998-11-04 | 4 |
| 1998-11-05 | 1 |
| 1998-11-13 | 2 |

In this example, it looks like there are two questionable dates in January 1998 and no other messages until October, at which point, the messages start coming in more regularly. This seems weird, and it also points to another issue, is it important that we have every date on the x axis, even if there were no e-mails sent that day?