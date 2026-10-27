By analyzing the resulting dataset, we can see that the Netflix app was launched, a /media/nowPlaying event was triggered, and the device was rotated into landscape mode. To understand what was playing on Netflix, we need to analyze the metadata associated with the event. In this case, the associated ID is 51732, which we will use to query the ZSTRUCTUREDMETADATA table:

```
SELECT
Z_PK AS "ID",
Z_DKNOWPLAYINGMETADATAKEY__TITLE
FROM ZSTRUCTUREDMETADATA
WHERE "ID" = "51732";
```

The result, shown in the following screenshot, displays the name of the application that was playing, Netflix, and the name of the movie I was watching:

![](bbox=[0.25, 0.44, 0.74, 0.6])

Figure 5.10 – The metadata associated with the /media/nowPlaying event

As you can see from these simple examples, the investigator has the ability to analyze the user's behavior in great detail and understand not only what application the user was using but also what exactly the user was doing with a particular application. For example, by customizing SQL queries, it is possible to learn what website the user visited or what PDF file the user was reading. We will look at these use cases in detail over the course of the next chapters.

Tip