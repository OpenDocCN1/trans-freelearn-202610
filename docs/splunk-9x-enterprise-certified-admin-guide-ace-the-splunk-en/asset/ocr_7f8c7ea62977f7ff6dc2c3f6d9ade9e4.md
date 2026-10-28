```
NO_BINARY_CHECK=true
CHARSET=AUTO
TIME_PREFIX=^
TIME_FORMAT=%Y-%m-%d %H:%M:%S
SEDCMD-maskphone  = s/\(\d{3}\)\s\d{3}-\d{4}/XXXXXXXX/g
```

We get the following screen:

![](bbox=[0.13, 0.235, 0.865, 0.405])

# Summary

This whole chapter revolved around the props.conf and transforms.conf settings up until the end and was purely technical. We began by understanding the parsing phase, coming right after the input phase, and its significance. Out of the three components, the full parsing pipeline exists on the HF and the indexer, and not on the UF; however, the UF is able to parse structured files through the INDEXED_EXTRACTIONS setting. We learned that it is mandatory to deploy parsing settings on the HF if the indexers are fronted by it.

Afterward, we looked at the props.conf stanzas related to sourcetype, source, and host and went through the specification of the source type definition for line breaker, line merging, and timestamp identification. To continue, we learned about the transforms.conf stanzas that work in accordance with props.conf. We further advanced into applying the SEDCMD setting via props.conf for data masking and the transforms.conf settings for overriding the source type, index routing, and filtering unwanted events using nullQueue.

Finally, using the Splunk data preview feature, we tested the new source type definition settings and the data masking setting, SEDCMD.

In the upcoming chapter, we will delve into the topic of fields and explore how to create both search-time and index-time fields. Additionally, we will focus on lookups and their creation using Splunk Web and the Splunk App for Lookup File Editing.