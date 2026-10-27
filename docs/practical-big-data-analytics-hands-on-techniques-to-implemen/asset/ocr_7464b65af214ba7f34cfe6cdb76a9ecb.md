In order to combine the data in laureate.json with country-specific information, we need to download the countryInfo.txt from geonames.org We will now download the second file that we need for the exercise, country.json. We will use both laureates.json and country.json for the exercise.

### country.json: Download it from http://www.geonames.org (license: https://creativecommons.org/licenses/by/3.0/). Modify the start and end of the JSON string to import into MongoDB as shown as follows:

```
# The file country.json contains descriptive information about all countries
# We will use this file for our tutorial
```

### Download country.json

```
(cloudera@quickstart mongodata)$ curl -o country.json "https://raw.githubusercontent.com/xbsd/packtbigdata/master/country.json"
% Total    % Received % Xferd  Average Speed   Time    Time     Time
Current
                           Dload  Upload   Total   Spent    Left
Speed
100 113k 100 113k    0     0  360k      0 --:--:-- --:--:-- --:--:--
885k
```

### The file, country.json has already been cleaned and can be imported directly into MongoDB

```
(cloudera@quickstart mongodata)$ mongoimport --jsonArray --db nobel --collection country --file country.json
2017-07-30T12:10:35.554-0700    connected to: localhost
2017-07-30T12:10:35.580-0700    imported 250 documents
```

### MONGO SHELL ###

```
(cloudera@quickstart mongodata)$ mongo
MongoDB shell version v3.4.6
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.6
Server has startup warnings:
(...)

2017-07-30T10:52:17.298-0700 I CONTROL  (initandlisten)
```

### Switch to the database nobel using the 'use <databasename>' command

```
> use nobel
switched to db nobel
```

### Show all collections (i.e., tables)