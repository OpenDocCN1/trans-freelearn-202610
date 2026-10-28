# Dealing with Date, DateTime, and DateTimeZone

Generating date, datetime, and datetimezone values in Power Query is simple. We just need to use one of the following three functions:

+   1. To generate date values, we can use the following command:

```
#date(year as number, month as number, day as number)
```

+   2. To generate datetime values, we can use the following command:

```
#datetime(year as number, month as number, day as number, hour as number, minute as number, second as number)
```

+   3. To generate datetimezone values, we can use the following command:

```
#datetimezone(year as number, month as number, day as number, hour as number, minute as number, second as number, offsetHours as number, offsetMinutes as number)
```

The following code generates a record of the Date, DateTime, and DateTimeZone values:

```
let
    Source = [
        Date = #date(2020, 8, 9)
        , DateTime = #datetime(2020, 8, 9, 17, 0, 0)
        , DateTimeZone = #datetimezone(2020, 8, 9, 17, 0, 0, 12, 0)
    ]
in
    Source
```

The results of the preceding code are illustrated in the following image: