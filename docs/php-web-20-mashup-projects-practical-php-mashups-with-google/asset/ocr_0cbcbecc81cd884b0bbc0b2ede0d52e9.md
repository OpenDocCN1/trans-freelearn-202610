# Lines Query

This one is easier than it may first appear. Our lines query must get all of the lines in the system. However, the RDF file does not have a section of just lines. It does have the section where it describes all of the connections in a line, though. We can simply grab all of these connection items and use the DISTINCT keyword on the line name to make sure we only get one of each.

```
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
FROM <tube_model2.rdf>
SELECT DISTINCT ?lineName
WHERE {
    ?type rdfs:type <http://space.frot.org/rdf/space.owl#Tube_Line> .
    ?type dc:title ?lineName
}
ORDER BY ?lineName';
```

The WHERE clause by itself, gets all of the line names from the dc:title element based on a type/resource combination like the previous query. However, the DISTINCT keyword filters out all the repeat instances.

# Lines to Stations Query

Remember previously that RDF items do not have relationships like SQL does per se. We can work around this by using queries to find the subject of the child object. We will have to do this to map the relationship between lines and stations.

```
PREFIX : <http://xmlns.com/foaf/0.1/>
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?lineName ?stationName
FROM <tube_model2.rdf>
WHERE {
    ?line rdfs:type <http://space.frot.org/rdf/space.owl#Tube_Line> .
    ?line dc:title ?lineName .
    ?line rdf:subject ?infourl .
    ?infourl dc:title ?stationName
}
ORDER BY ?lineName ?stationName';
```