places each results object into a RAP resource object named $station. For each station in the results set, a new station is instantiated. Using the setter methods, the results in $station populate each station object's name, latitude, and longitude. It then places this object into the array to be returned, with the name of the station as the key. Without any integer identifiers in RDF, we are going to have to use the next best thing. The names of the stations and lines are going to have to be the keys.

The same principle applies to getAllLines(), which grabs all of the station lines in the RDF document.

```
function getAllLines(&$sparqlClient) {
    $returnArray = array();
    $query = '
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
FROM <tube_model2.rdf>
SELECT DISTINCT ?lineName
WHERE {
    ?type rdfs:type
        <http://space.frot.org/rdf/space.owl#Tube_Line> .
    ?type dc:title ?lineName
}
ORDER BY ?lineName';
    $result = $sparqlClient->sparqlQuery($query);
    if ($result != "false") {
        foreach ($result as $line) {
            if ($line != "") {
                $lineObj = new Line();
                $lineObj->setLineName($line['?lineName']->getLabel());
                $returnArray[$line['?lineName']->getLabel()] = $lineObj;
            }
        }
    }
    return $returnArray;
}
```

The same principle applies to getAllLines(), which grabs all of the station lines in the RDF document. Again, the line name is the key in this array.