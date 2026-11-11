This query is asking for a line name and a station name pairing. All stations that belong to a line should be included with that line. If a line has twelve stations, there should be twelve entries with that line in the result set, with each station having on entry in that pair.

The first line in the WHERE clause is simple enough. It sets the subject of all Tube lines in the ?line variable. The second line sets the ?lineName variable, which we want to extract, by making it the object of the dc:title predicate. It gets interesting in the third line.

In the RDF document, these connection items have a subject element.

```
<rdf:subject rdf:resource="http://london.openguides.org/index.cgi?id=Wapping_Station;format-rdf#obj"/>
```

These subject elements tell us that subject of this connection item is the resource attribute value. The third line in the WHERE clause, then, sets the rdf:resource value in a variable named ?infoUrl. Remember earlier when we looked at the stations we noted the rdf:about attribute in the Description elements of the stations could be used as a unique identifier for those stations? This is where it comes in handy. These identifiers are used in rdf:resource in these connection items.

In the fourth line, we use this station identifier URL as the subject to grab the station name. This fourth line looks for all subjects with the unique station URL and operates on those items. In other words, it looks back at the station items in the first half of the page.

Finally, back in the SELECT statement, we add a DISTINCT keyword to ?lineName. This is because a connection between two stations is actually represented twice in our document. You'll find a statement that says, "Station A is connected to Station B", and later on in the document, you'll find "Station B connects to Station A". This is no accident, but will cause each connection to be listed twice. DISTINCT will eliminate that.

We have successfully worked around the issue of relationships. Though your classic foreign key constraints in SQL are not available, we do have identifiers in this file that we can play with. Fortunately, we have a well designed document, but this may not always be the case. You may have to query more than one document, or you may have to get extra complicated with your SPARQL WHERE clauses.

# Database Population Script

Now that we have our SPARQL queries, it's time to actually use them to populate our database. We will write a procedural script that uses RDF API for PHP to do just that.