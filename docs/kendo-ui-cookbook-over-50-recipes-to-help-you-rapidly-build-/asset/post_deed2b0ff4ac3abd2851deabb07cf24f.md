
The geoJSON data mentioned contains the coordinates that correspond to the points on
the map. These points will be used to construct vector shapes. The geoJSON data also lists
other properties such as NAME and CENSUSAREA. Let's use the previous JSON data as a
DataSource object in building the map using the kendoMap function:

```javascript
$('#chart').kendoMap({
center: [30.2681, -97.7448],
zoom: 4,
layers: [{
type: 'shape',
dataSource: {
type: 'geojson',
transport: {
read: './geojson-us.json'
}
}
}]
});
```

Here, the type attribute is set as shape and dataSource is specified, which refers to
the file that contains the geoJSON data. The framework would parse the geoJSON data and
refer to the coordinates to construct the map.

We can use other properties present in the geoJSON data and add some functionality to the
map. For example, we could convert the CENSUSAREA field to a hex number and use this to
fill the shape:

```javascript
$('#chart').kendoMap({
center: [30.2681, -97.7448],
zoom: 4,
layers: [{
type: 'shape',
dataSource: {
type: 'geojson',
transport: {
read: './geojson-us.json'
}
}
}]),
shapeCreated: function (e) {
var shapeColor = parseInt(e.shape.dataItem.properties.
CENSUSAREA, 16).toString();
shapeColor = '#' + ('FFFFFF' + shapeColor).slice(-6);
e.shape.fill(shapeColor);
}
});
```

In the preceding code snippet, the shapeCreated function is added, which is called whenever a shape is added to the map.

## How it works...

When you render the page, you will see a map created that will contain shapes, as shown in the following screenshot:

![](img/2a66507445090790d34dc5588b56b24c_238_0.png)

After adding colors to shapes using the shapeCreated function, you will see a colored map.

![](img/2a66507445090790d34dc5588b56b24c_238_1.png)

The shape is calculated by converting the CENSUSAREA field value to a hex number and then prefixing it to make it a fixed length.

## Index

### A

- **ActionSheet widget**
  - about 130
  - used, for displaying list of actions 150-153
- **activate event 86**
- **advance charting widgets**
  - used, for data visualization 193
- **aggregate functions**
  - avg function 183
  - count function 183
  - first function 183
  - max function 183
  - min function 183
  - used, to display data on charts 181-184
- **Ajax**
  - used, for loading content of modal window 104
  - used, for loading PanelBar content 83-86
- **Application object**
  - used, for navigating between multiple views 121-125
- **autoUpload property 95**
- **avg function 183**

### B

- **barcode image**
  - generating, various encoding methods used 207, 208
- **built-in Validator**
  - used, for validating user input 19-22

### C

- **cancel event handler 99**
- **center method 108**
- **charts**
  - binding, to DataSource object 173-175
  - creating, kendoChart function used 168, 169
  - creating to display data, aggregate functions used 181-184
  - default theme, modifying 172, 173
  - legend information position, modifying 171
  - making interactive, event handlers used 185-188
  - multiaxis chart, creating 177-181
  - multiple categories, displaying 170
  - multiple series, displaying 175
  - title position, modifying 171
  - values, displaying 176, 177
- **charts, options**
  - categoryAxis option 169
  - series option 169
  - title option 169
  - valueAxis option 169
- **chart type**
  - changing, dynamically 188-192
- **checkbox**
  - used, for selecting nodes in TreeView 58-61
- **click event handler 107**
- **client-side templates**
  - used, for generating HTML markup 8-11
- **close method 108**
- **complete event handler 99**
- **connect function 213**

Content Delivery Network (CDN) 8
contentLoad event 86
content, PanelBar
loading, Ajax used 83-86
contentUrls property 84
count function 183
custom aggregator function 183
customization, PanelBar 88, 89
customization, Window 105-108

### D

data
displaying, in Grid 28-32
displaying in Grid, filters used 35-38
sorting, in Grid 32-35
data-bind attribute 18, 189
data binding
creating, MVVM used 12-16
used, for generating HTML content 16-19
data-icon attribute 133
data-id attribute 113
data-layout attribute 114
data-role attribute 113, 131
DataSource component 28
DataSource object
about 29, 52
charts, binding to 173-175
ListView widget, binding to 134-136
PanelBar, binding to 86, 88
dataSource property 138
data-style attribute 132
dataTextField attribute 54
dataTransition attribute
about 125
fade 126
overlay 126
slide 125
zoom 125
DataViz (data visualization)
about 167
with advance charting widgets 193
delete command 39
display of list items
customizing, template used 93, 94
Django 8
doubletap event 128

drag-and-drop
used, for moving elements in TreeView 55-58
used, for uploading files 97

edit command 39
editing
in Grid 39
inline 39
popup 39
Editor
about 65
image browser tool used, for inserting
images 70-76
Editor toolset
tools, adding in 67-70
tools, removing in 67-70
endless scrolling list
building, ListView widget used 148-150
error event handler 86, 99
event handlers
used, for making charts interactive 185-188
exec function 70
expand event 86

### F

field property 29
files
uploading asynchronously, file uploader
used 94-96
uploading, drag-and-drop used 97
uploading synchronously, file uploader
used 91-93
file uploader
about 91
file upload events, listening 98, 99
localizing 97
used, for uploading files
asynchronously 94-96
used, for uploading files synchronously 91-93
file upload events
cancel event handler 99
complete event handler 99
error event handler 99
listening 98, 99

progress event handler 99
remove event handler 99
select event handler 99
success event handler 99
upload event handler 99
filters
used, for displaying data in Grid 35-38
first function 183
fixed headers
creating, ListView widget used 137-139
fixedHeaders property 138
flow diagrams
creating, Kendo Diagram used 211-215

### G
GeoJSON 225
getKendoDiagram function 211
Grid
about 23
creating, to display tabular data 23-27
customizing 46-48
data displaying, filters used 35-38
data, displaying in 28-32
data, sorting in 32-35
records, creating 39-43
records, deleting 39-43
records, updating 39-43
virtualization, using 43-45
group property 138

### H
Handlebars 8
hash templates 8
hasModels property 144
hierarchical list
building, ListView widget used 143-147
hierarchical structural diagrams
creating, layout option used 215-220
hold event 128
href attribute 131
HTML content
generating, data binding used 16-19
HTML markup
generating, client-side templates used 8-11
http method 41

### I
icons
using with nodes, in TreeView 62, 63
id property 144
image browser tool
used, for inserting images into Editor 70-76
images
inserting into Editor, image browser tool
used 70-76
initial layout
setting, during application
initialization 117-121
initial view
setting, during application
initialization 117-121
inline editing 39
inserthtml command 70
items array 69

### J
Jade 8
Java Server Pages 8

### K
kendoChart function
used, for creating charts 168, 169
Kendo Diagram
used, for creating flow diagrams 211-215
kendoDiagram function
connectionDefaults option 211
endCap option 211
startCap option 211
kendoEditor function
about 66
used, for creating WYSIWYG Editor 65, 66
kendoMap function 220
kendoMobileListView function 135
kendoRadialGauge function
endAngle option 200
pointer option 200
startAngle option 200
kendoStockChart function 195
Kendo UI
application framework 7
data visualization 167

Editor 65
file uploader 91
Grid 23
Mobile framework: 111
Mobile widgets: 129
PanelBar 77
TreeView 49
Window 101

### Kendo UI application framework

client-side templates used, for generating
HTML markup 8-11
data binding, creating with MVVM 12-16
data binding used, for generating HTML
content 16-19
user input validating, built-in Validator
used 19-22
kendoValidator method 22

### L

labels option 176
layout
creating, for mobile application 112-116
layout option, kendoDiagram function
left subtype 217
mindmapHorizontal subtype 217
mindmapVertical subtype 217
radial subtype 217
right subtype 217
tipOver subtype 217
up subtype 217
used, for creating hierarchical structural
diagrams 215-220
Linear Gauge widget
creating 205-207
list
creating, ListView widget used 130-134
list elements
filtering, ListView widget used 139-143
ListView widget
about 130-134
binding, to DataSource object 134-136
list elements, filtering 139-143
used, for building endless scrolling
list 148-150
used, for building hierarchical list 143-147

used, for creating fixed headers 137-139
used, for creating list 130-134

### M

map
creating, OpenStreet map used 220-224
creating, that is bound to GeoJSON
data 225-227
maximize method 108
min function 183
minimize method 108
Mobile framework
about 111
initial layout, setting 117-121
initial view, setting 117-121
layout, creating for mobile
application 112-116
multiple views, navigating between 121-125
touch events, adding to mobile
application 126-128
views, adding to layout 112-116
Mobile library. See Mobile widgets
mobileListViewInit function 138
Mobile widgets
about 129
ActionSheet widget 129
ListView widget 129
Navbar widget 129
ScrollView widget 129
SplitView widget 130
TabStrip widget 129
modal window
configuring, to display pop up 102, 103
content loading, Ajax used 104
differentiating, with Window 102
displaying, on click of button 103, 104
Model View Controller. See MVC
Model View Presenter. See MVP
Model View ViewModel. See MVVM
multiaxis chart
creating 177-181
multiple file uploads
disabling 93
multiple views
navigating between, Application object
used 121-125
