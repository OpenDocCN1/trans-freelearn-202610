
## CSV to JSON using Python

We have found several ways to convert CSV files to JSON using Python. The first of these uses the built-in csv and json libraries. Suppose we have a CSV file that has rows like this (only the first three rows shown):

```
name,email_id
"Lysa Akin",lysa.akin@enron.com
"Phillip Allen",k..allen@enron.com
"Harry Arora",harry.arora@enron.com
```

We can write a Python program to read these rows and convert them to JSON:

```
import json
import csv

# read in the CSV file
with open('enronEmail.csv') as file:
    file_csv = csv.DictReader(file)
    output = '['
    # process each dictionary row
    for row in file_csv:
        # put a comma between the entities
        output += json.dumps(row) + ','
    output = output.rstrip(',') + ']'
# write out a new file to disk
f = open('enronEmailPy.json','w')
f.write(output)
f.close()
```

The resulting JSON will look like this (only the first two rows are shown):

```
[{"email_id": "lysa.akin@enron.com", "name": "Lysa Akin"},
{"email_id": "k..allen@enron.com", "name": "Phillip Allen"},...]
```

One nice thing about using this method is that it does not require any special installations of libraries or any command-line access, apart from getting and putting the files you are reading (CSV) and writing (JSON).

## CSV to JSON using csvkit

The second method of changing CSV into JSON relies on a very interesting Python toolkit called **csvkit**. To install csvkit using Canopy, simply launch the Canopy terminal window (you can find it inside Canopy by navigating to **Tools | Canopy Terminal**) and then run the `pip install csvkit` command. All the dependencies for using csvkit will be installed for you. At this point, you have the option of accessing csvkit via a Python program as a library using `import csvkit` or via the command line, as we will do in the following snippet:

```
csvjson enronEmail.csv > enronEmail.json
```

This command takes a `enronEmail.csv` CSV file and transforms it to a JSON `enronEmail.csvkit.json` file quickly and painlessly.

There are several other extremely useful command-line programs that come with the csvkit package, including `csvcut`, which can extract an arbitrary list of columns from a CSV file, and `csvformat`, which can perform delimiter exchanges on CSV files or alter line endings or similar cleaning procedures. The `csvcut` program is particularly helpful if you want to extract just a few columns for processing. For any of these command-line tools, you can redirect its output to a new file. The following command line takes a file called `bigFile.csv`, cuts out the first and third column, and saves the result as a new CSV file:

```
csvcut bigFile.csv -c 1,3 > firstThirdCols.csv
```

![](img/9aa003afabe05fdb07da46ad71325de6_114_0.png)

Additional information about csvkit, including full documentation, downloads, and examples, is available at http://csvkit.rtfd.org/.

## Python JSON to CSV

It is quite straightforward to use Python to read in a JSON file and convert it to CSV for processing:

```
import json
import csv

with open('enronEmailPy.json', 'r') as f:
    dicts = json.load(f)
out = open('enronEmailPy.csv', 'w')
writer = csv.DictWriter(out, dicts[0].keys())
writer.writeheader()
writer.writerows(dicts)
out.close()
```

This program takes a JSON file called `enronEmailPy.json` and exports a CSV-formatted version of this file using the keys for the JSON as the header row in the new file, called `enronEmailPy.csv`.

## The example project

In this chapter, we have focused on converting data from one format to another, which is a common data cleaning task that will need to be done time and again before the rest of the data analysis project can be completed. We focused on some very common text formats (CSV and JSON) and common locations for data (files and SQL databases). Now, we are ready to extend our basic knowledge of data conversions with a sample project that will ask us to make conversions between some less standardized — but still text-based — data formats.

In this project, we want to investigate our Facebook social network. We will:

- 1. Download our Facebook social network (friends and relationships between them) using netvizz into a text-based file format called **Graph Description Format (GDF)**.
- 2. Build a graphical representation of a Facebook social network showing the people in our network as nodes and their friendships as connecting lines (called *edges*) between these nodes. To do this, we will use the D3 JavaScript graphing library. This library expects a JSON representation of the data in the network.
- 3. Calculate some metrics about the social network, such as the size of the network (known as the *degree* of the network) and the shortest path between two people our network. To do this, we will use the `networkx` package in Python. This package expects data in a text-based format, called the **Pajek** format.

The primary goal of this project will be to show how to reconcile all these different expected formats (GDF, Pajek, and JSON) and perform conversions from one format to another. Our secondary goal will be to actually provide enough sample code and guidance to perform a small analysis of our social network.

### Step one – download Facebook data as GDF

For this step, you will need to be logged into your Facebook account. Use Facebook's search box to find the netvizz app, or use this URL to directly link to the netvizz app: `https://apps.facebook.com/netvizz/`.

Once on the netvizz page, click on **personal network**. The page that follows explains that clicking on the **start** button will provide a downloadable file with two items in it: a GDF format file that lists all your friends and the connections between them and a tab-delimited **Tab Separated Values (TSV)** stats file. We are primarily interested in the GDF file for this project. Click on the **start** button, and on the subsequent page, right-click on the GDF file to save it to your local disk, as shown in the following screenshot:

![](img/9aa003afabe05fdb07da46ad71325de6_116_0.png)

The netvizz Facebook app allows us to download our social network as a GDF file

It may be helpful to also give the file a shorter name at this point. (I called my file `personal.gdf` and saved it in a directory created just for this project.)

### Step two – look at the GDF file format in a text editor

Open the file in your text editor (I am using Text Wrangler for this), and note a few things about the format of this file:

- 1. The file is divided into two parts: nodes and edges.
- 2. The nodes are found in the first part of the file, preceded by the word `nodedef`. The list of nodes is a list of all my friends and some basic facts about them (their gender and their internal Facebook identification number). The nodes are listed in the order of the date when the person joined Facebook.
- 3. The second part of the file shows the edges or connections between my friends. Sometimes, these are also called links. This section of the file is preceded by the word `edgedef`. The edges describe which of my friends are linked to which other friends.

Here is an excerpt of what a nodes section looks like:

```
nodedef>name VARCHAR,label VARCHAR,sex VARCHAR,locale VARCHAR,agerank INT
1234,Bugs Bunny,male,en_US,296
2345,Daffy Duck,male,en_US,295
3456,Minnie Mouse,female,en_US,294
```

Here is an excerpt of what an edges section looks like. It shows that Bugs (1234) and Daffy (2345) are friends, and Bugs is also friends with Minnie (3456):

```
edgedef>node1 VARCHAR,node2 VARCHAR
1234,2345
1234,3456
3456,9876
```

### Step three – convert the GDF file into JSON

The task we want to perform is to build a representation of this data as a social network in D3. First, we need to look at the dozens of available examples of D3 to build a social network, such as those available in the D3 galleries of examples, https://github.com/mbostock/d3/wiki/Gallery and http://christopheviau.com/d3list/.

These examples of social network diagrams rely on JSON files. Each JSON file shows nodes and the edges between them. Here is an example of what one of these JSON files should look like:

```
{"nodes": [
  {"name":"Bugs Bunny"},
  {"name":"Daffy Duck"},
  {"name":"Minnie Mouse"}],
"edges": [
  {"source": 0,"target": 2},
  {"source": 1,"target": 3},
  {"source": 2,"target": 3}]}
```

The most important thing about this JSON code is to note that it has the same two main chunks as the GDF file did: nodes and edges. The nodes are simply the person's name. The edges are a list of number pairs representing friendship relations. Instead of using the Facebook identification number, though, these pairs use an index for each item in the nodes list, starting with 0.
