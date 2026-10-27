+   - The Module column indicates the name of the module that was responsible for extracting and analyzing the event.

Typically, the output command will contain most of the interesting data from a forensic perspective. The following figure shows what the JSON object looks like for a `Battery Level` event:

```
```json
{
  "ADJUSTED_TIMESTAMP": "2021-06-19 07:57:09",
  "LEVEL": 31.0,
  "RAW_LEVEL": 31.2364696962684408,
  "IS_CHARGING": 0,
  "FULLY_CHARGED": 0,
  "ORIGINAL_TIMESTAMP": "2021-04-26 05:35:55",
  "OFFSET_TIMESTAMP": "2021-04-28 15:48:56",
  "TIME_OFFSET": 4674074.367777824,
  "PLBATTERYAGENT_EVENTBATTERY_TABLE_ID": 480012
}
```
```

Figure 5.13 – The JSON object stored in Apollo's Output column

The beauty of Apollo is that it is a fully modular and customizable tool that allows the investigator to extract all pattern-of-life data or choose to only include some artifacts by using the appropriate modules. Condensing the result into a single file means that the investigator gains the ability to view a timeline of events that can be easily filtered down based on date and time or by searching for a specific event.

Apollo is under active development and modules are continually being added and updated. Finally, it should be noted that the tool not only supports iOS but also allows the analysis of pattern-of-life data from MacOS devices and others.

# Summary

In this chapter, we learned what pattern-of-life forensics is and how an investigator can benefit from analyzing such data to visualize user behavior and detect abnormal patterns.