
Now that we have mapped the variable sized symbol to the natural log of the count of students traveling on each segment, we will see a pleasing visualization of the "flow" of students traveling on each road segment. The student layer, showing the student locations, is displayed alongside the flow to better illustrate what the flow visualization shows.

![](img/2e88bf5938bf48a409c0a534a8775b21_326_0.png)

### Web applications – creating safe corridors

Decision makers can use the accumulated shortest paths output to identify the busiest paths to the school. They can use this information to communicate with guardians about the safest routes for their children.

Planners can go a step further by investing in safe infrastructure for the most used paths. For example, planners can identify the busy crossings over highways using the "count" attribute (and visualization) from the query layer and the "highways" attribute from newark_osm.

Of course, communication with stakeholders is ideally a two-way process. To achieve this goal, planners could establish a social networking account, such as a Twitter account, for parents and students to report the problems or features of the walking routes. Planners would likely want to look at this data as well to adjust the safe routes to the problem spots or amenities. This highly simplistic model should be adjusted for the other variables that could also be captured in the data and modeled, such as high traffic roads and so on.

#### Registering a Twitter account and API access

The following instructions will direct you on how to set up a new Twitter account in a web browser and get your community to make geotagged tweets:

1. Create a Twitter account for this purpose (a nonpersonal one). The account will need to be linked to a unique mobile phone and e-mail. If you've already linked your e-mail and mobile phone with an account, there are some hints for getting around this in the following section.

2. Go to https://apps.twitter.com/ and create a new app.

3. You will need to unregister your phone number if it is already registered with another account (Twitter will warn you about this).

4. In Application Settings, find manage keys and access tokens. Here, you will find your consumer key and secret.

5. You must also create an access token by clicking on Create my Access Token.

6. Users who would like to have their tweets added to the system should be directed to use your Twitter handle (@YOURNAME). Retweet the tweets that you wish to add to your map. For a more passive solution, you can also follow all the users who you wish to capture; although, you'll need to find some way to filter out their irrelevant tweets.

#### Setting up the Twitter Tools API

We must now download and install Python-based Twitter Tools, which leverage the Twitter API. This will allow us to pull down GeoJSON from our Twitter account. Perform the following steps:

1. Download the Twitter Tools API from GitHub: https://github.com/sixohsix/twitter.

2. Open the OSGeo4W shell using the **Run as administrator** command via the context menu, or if you're on Mac or Linux, use `sudo` to run it with full privileges. Your OS Account must have administrator privileges. Navigate to `C:\Program Files\QGIS Wien\OSGeo4W.bat`.

3. Extract the Twitter Tools API code and change directory (`cd`) into the directory that you extracted into (for example, `C:\Users\[YOURUSERNAME]\Downloads\twitter-master\twitter-master`), using the following command line:

```
> cd C:\Users\[YOURUSERNAME]\Downloads\twitter-master\twitter-master
```

4. Run the following from the command line to install the Twitter Tools software and dependencies:

```
> python setup.py install
> twitter
```

5. Accept the authorization for the command-line tools. You will need to copy and paste a PIN (as given) from the browser to the command line.

6. Exit the command-line shell and start another OSGeo4W shell under your regular account.

7. You can use `twitter --help` for more options. Execute the following in the command line:

```
> twitter --format json 1> "C:\packt\c4\data\output\twitter.json"
> cd c:\packt\c4
> python
```

> Running `python setup.py install` in a directory containing the `setup.py` file on a path including the Python executable is the normal way to build (install) a Python program. You will need to install the `setuptools` module beforehand. The instructions to do so can be found on this website: https://pypi.python.org/pypi/setuptools.

8. Run the following in the interpreter (refer to the following section to run it noninteractively):

```
import json
f = open('./output/twitter.json', 'r')
jsonStr = f.read()
f.close()
jpy = json.loads(jsonStr)
geojson = ''
for x in jpy['safe']:
    if x['geo'] :
        geojson += '{"type": "Feature","geometry": {"type": "Point", "coordinates": [' + str(x['geo']['coordinates'][1]) + ',' + str(x['geo']['coordinates'][0]) + ']}', "properties": {"id": "' + str(x['id']) + '", "text": "' + x['text'] + '"}}',
geojson = geojson[:-1]
geojson += '}'
geojson = '{"type": "FeatureCollection","features": [' + geojson
f = open('./data/output/twitter.geojson', 'w')
f.write(geojson)
f.close()
```

Or run the following command:

```
> python twitterJson2GeoJson.py
```

Here is an example of the GeoJSON-formatted output:

```
{"type": "FeatureCollection","features": [{"type": "Feature","geometry": {"type": "Point", "coordinates": [-75.75451,39.67434]}, "properties": {"id": "606454366212530177", "text": "Hello world"}},{"type": "Feature","geometry": {"type": "Point", "coordinates": [-75.73968,39.68139]}, "properties": {"id": "606454626456473600", "text": "Testing"}},{"type": "Feature", "geometry": {"type": "Point", "coordinates": [-75.76838,39.69243]}, "properties": {"id": "606479472271826944", "text": "Test"}}]}
```

Save this as c4/output/twitter.geojson from a text editor and import the file into QGIS as a vector layer to preview it along with the other layers. When these layers are symbolized, you may see something similar to the following image:

![](img/2e88bf5938bf48a409c0a534a8775b21_330_0.png)

## 5 Demonstrating Change

In this chapter, we will encounter the visualization and analytical techniques of exploring the relationships between place and time and between the places themselves.

The data derived from temporal and spatial relationships is useful in learning more about the geographic objects that we are studying – from hydrological features to population units. This is particularly true if the data is not directly available for the geographic object of interest: either for a particular variable, for a particular time, or at all.

In this example, we will look at the demographic data from the US Census applied to the State House Districts, for election purposes. Elected officials often want to understand how the neighborhoods in their jurisdictions are changing demographically. Are their constituents becoming younger or more affluent? Is unemployment rising? Demographic factors can be used to predict the issues that will be of interest to potential voters and thus may be used for promotional purposes by the campaigns.

In this chapter, we will cover the following topics:

- Using spatial relationships to leverage data
- Preparing data relationships for static production
- Vector simplification
- Using TopoJSON for vector data size reduction and performance
- D3 data visualization for API
- Animated time series maps

### Leveraging spatial relationships

So far, we've looked at the methods of analysis that take advantage of the continuity of the gridded raster data or of the geometric formality of the topological network data.

For ordinary vector data, we need a more abstract method of analysis, which is establishing the formal relationships based on the conditions in the spatial arrangement of geometric objects.

For most of this section, we will gather and prepare the data in ways that will be familiar. When we get to preparing the boundary data, which is leveraging the State House Districts data from the census tracts, we will be covering new territory – using the spatial relationships to construct the data for a given geographic unit.

### Gathering the data

First, we will gather data from the sections of the US Census website. Though this workflow will be particularly useful for those working with the US demographic data, it will also be instructive for those dealing with any kind of data linked to geographic boundaries.

To begin with, obtain the boundary data with a unique identifier. After doing this, obtain the tabular data with the same unique identifier and then join on the identifier.

#### Boundaries

Download 2014 TIGER/Line Census Tracts and State Congressional Districts from the US Census at https://www.census.gov/geo/maps-data/data/tiger-line.html.

1. Select 2014 from the tabs displayed; this should be the default year.

2. Click on the Download accordion heading and click on Web interface.

3. Under Select a layer type, select Census Tracts and click on submit; under Census Tract, select Pennsylvania and click on Download.
