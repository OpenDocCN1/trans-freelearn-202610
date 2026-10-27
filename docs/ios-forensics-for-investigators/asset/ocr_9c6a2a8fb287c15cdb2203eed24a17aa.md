643507200 timestamp and the 643680000 timestamp. To do so, run the following query:

```
SELECT
datetime(ZOBJECT.ZSTARTDATE+978307200, 'UNIXEPOCH',
'LOCALTIME') as "START",
datetime(ZOBJECT.ZENDDATE+978307200, 'UNIXEPOCH',
'LOCALTIME') as "END",
ZOBJECT.ZSECONDSFROMGMT/3600 AS "GMT OFFSET",
(ZOBJECT.ZENDDATE-ZOBJECT.ZSTARTDATE) as "USAGE IN SECONDS",
ZOBJECT.ZSTREAMNAME,
ZOBJECT.ZVALUESTRING
FROM ZOBJECT
WHERE ZSTREAMNAME IS "/app/inFocus"
AND ZOBJECT.ZSTARTDATE > 643507200
AND ZOBJECT.ZSTARTDATE < 643680000
ORDER BY "START";
```

First, we use the datetime SQL command to select the start and end timestamps and convert them to a readable string. Then, we calculate the GMT offset in hours and the duration of the event in seconds. Finally, we filter down the results by only selecting /app/inFocus events that occurred between the timestamps we calculated previously.

The resulting dataset is shown below:

![](bbox=[0.12, 0.59, 0.87, 0.8])

Figure 5.7 – The query result shows application usage