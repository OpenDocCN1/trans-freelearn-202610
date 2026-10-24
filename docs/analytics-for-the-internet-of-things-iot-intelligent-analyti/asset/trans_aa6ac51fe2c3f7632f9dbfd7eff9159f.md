

Dashboards can also be created for this purpose by following the same process we introduced in this chapter. Think about what you want to watch out for and set up an alert view to identify it for you.

## Alert principles

There are some principles to follow when designing an alert system, even a simple one that will be part of a dashboard:

- **Balance alert sensitivity to minimize false positives**: People will learn quickly to ignore alerts if they rarely identify an actual problem. This has to be balanced against missing too many real issues though. A cost benefit calculation can be used to help in this decision.
- [*cost of investigating a problem*] * [*number of alerts*] < [*cost of actual problem*] * [*probability of true positive*]
- The left side of that equation should be noticeably less than the right side.
- **Be wary of alert fatigue**: A long list of continuous alerts is daunting, and human brains tend to become conditioned over time to recognize it as a normal situation. Alerts will start to become background noise and will not get a response. Anyone with an Android smartphone knows how this feels - too many alerts are exhausting. There does not need to be an alert for every problem. Keep it to a manageable amount for the big ones.
- **Make alerts like a to-do list**: A user should not have to search for where the problem is; it should be listed out for them if there are any. If there are no issues discovered, a blank list can be comforting - like leaving work an hour early!
- **Incorporate a tracking system for alert responses**: Nothing is more frustrating than spending a couple of hours investigating a problem someone else has already discovered and corrected.

## Organizing alerts using a Tableau dashboard

A simple example can highlight how to use a dashboard for alerting purposes. Tableau server has an email functionality that a user can subscribe to that will email an image of the dashboard at a regular interval with an embedded link back to the interactive dashboard. This can then be read and digested like a morning newspaper.

You learned while investigating the 15 minute precipitation dataset that there were measurement values that represented a state instead of an actual reading. In these cases, the value was either -9999 or 999.99. What we would want to see, if we were relying on this data on a daily basis, is that a station reports valid precipitation measurements and little to no non-measurement values. We can organize a simple question hierarchy to represent our thinking process:

![](img/9f2a55db1236f995dcd9392acbc82910_242_0.png)

Alert question hierarchy for stations reporting invalid precipitation data

We can create an alert dashboard to monitor this on a daily basis. This will allow quick identification of a problem. We will start by creating four additional calculated fields in the following order:

### Name: Precip Accumul Value Validity

### Calculation:

```
CASE [Qgag]
    WHEN -9999 THEN
        "Invalid"
    WHEN 999.990 THEN
        "Invalid"
    ELSE
        IF [Qgag] >= 0 AND [Qgag] <20 THEN
            "Valid"
        ELSE
            "Invalid"
        END
END
```

### Name: Precip Value Validity

### Calculation:

```
CASE [Amount of Precipitation (inches)]
    WHEN -9999 THEN
        "Invalid"
    WHEN 999.99 THEN
        "Invalid"
    ELSE
        IF [Amount of Precipitation (inches)] >= 0 AND [Amount of Precipitation (inches)] <20 THEN
            "Valid"
        ELSE
            "Invalid"
        END
END
```

### Name: Invalid Count

### Calculation:

```
IF [Precip Accumul Value Validity] = "Invalid" AND [Precip Value Validity] = "Invalid" THEN
    1
ELSE
    0
END
```

### Name: Valid Count

### Calculation:

```
If [Invalid Count] = 0 THEN
    1
ELSE
    0
END
```

## Summary

In this chapter, we discussed common mistakes when creating visuals for IoT data. Some tips were given to avoid making them. We introduced a method to develop dashboards and visualizations to communicate analytics to an audience. The method has the goal of aligning the visuals to the thought process of the person interacting with them.

We reviewed the use of position on a dashboard to convey importance. The most important piece of information should be in in the upper-left part for cultures that read left to right. Color can also be used effectively to highlight key information to the audience.

Tableau was used to demonstrate how to quickly create a dashboard to communicate your analytics in an interactive way. We walked through an example with the IoT weather data continued from a previous chapter. Some principles of alerting were reviewed along with an example using Tableau.

For further exploration, not discussed here but worthwhile for you to learn, there are also some great visualization packages in R and Python. The author recommends *ggplot* for R and *Seaborn* for Python.

## Applying Geospatial Analytics to IoT Data

"I know it has been tough since Willard left, but I have some good news; we will be getting you some help." These words are coming to you accompanied by a concerned, paternal look from the Vice President of Connected Product Development, who is standing in your cubicle.

Willard was your boss, now your former boss. He left recently to join another company as Head of their IoT division. They were highly impressed with his accomplishments building up a cloud-based IoT analytics capability, which was all your idea, of course, but *c’est la vie*.

Now, with your boss gone, the VP and other executives are worried about the momentum stalling. You are a little bit miffed at this since you had to twist your boss's arm to get him to go along with everything in the first place. *Look on the bright side*, you tell yourself, *at least you are getting some help*.

He continues, "We want to up the ante, none of the competition is aggregating their thermostat data and selling it to power companies. Maybe the new guy can figure out how to do that."

*Ha*, the new guy. "I got this, I developed what we have so far. I can certainly handle this," you state confidently, holding his gaze. "I've been looking for an excuse to do a little geospatial anyway."

You feel capable now; it is time they started giving you some credit. But you are not quite sure how to do it, as it seems a little daunting. The power companies are going to have service areas that do not stick to state boundaries or zip codes. You will need to somehow draw the service areas on a map and then find the devices in the area. This sounds like a lot of manual work.

"Well, we will see. I have lots of ideas if you can pull this one off," he replies. With this, he turns around and strolls assuredly down the hallway on his way to the next meeting. You hesitate for a second wondering if he knew all along that you were the brains behind the IoT analytics and just manipulated you into putting all your energy into his project. Then you shake your head thinking surely not, and get back to work.

This chapter is about leveraging the field of geospatial analysis to enhance IoT analytics. IoT devices are sometimes attached to equipment that moves around geographically. Even if this is not the case, devices will probably have diverse geographic locations when deployed. This creates an opportunity to find patterns and develop valuable services by using location based analysis. We will introduce some key concepts and technology for geospatial analytics.

This chapter covers the following topics:

- The benefit of geospatial analytics for IoT
- Geospatial analytics basics:
  - Welcome to Null Island
  - Coordinate Reference Systems
  - Python for geospatial analysis
- Vector-based methods:
  - A bounding box
  - Contains
  - Buffers
  - Simplify
  - The vector summary
- Raster-based methods
- Storing geospatial data:
  - File formats
  - Spatial extensions for relational databases
  - Geospatial data in HDFS
  - Spatial indexing

