
Your SPARQLer result set should look like this:

| name |
|------|
| "Pim Pim" |
| "Saffy" |
| "Manfred" |
| "Lizzie Borden" |
| "Tera-San" |
| "Moose" |
| "Hoser" |
| "Mathilda" |
| "Opal" |
| "Wozniak" |
| "Dolly" |
| "Avi" |
| "Snowball" |

## Querying for Types

In the first query, we used a literal value of the name `Saffy` to find what we were looking for. Simply searching on a literal value is often not a reliable approach. Earlier, we noted that the RDF Schema vocabulary allows us to classify subjects as RDF resources using the `type` element. This next example will show how to restrict on this element.

Let's say we wish to grab the names of all parrots. Our `WHERE` clause needs to do the following:

- Find the parrots in the RDF document.
- Extract their names.

The `type` element is still the predicate. However, this element does not have a value we can use as the triple object. Instead, the `resource` attribute value is the object in this triple. `resource` is a URI that points to a description of what a parrot is. Remember that triple objects can be either a literal value or a URI. Again, this particular example URI is only an example to identify, not a formal vocabulary definition, which it sometimes can be. This combination says "This subject is a parrot". From there, we can extract the `name` element as we did before.

The restriction requirement is similar to what we have been doing. The triple associated with it will use a URI instead of quoted literals like the previous examples. We can specify this simply by specifying the URI in the query using greater than/ less than signs.

This triple is simply this:

```
?Description rdfs:type <http://www.shuchow.com/#parrot>
```

Sometimes, you may find this resource attribute starts with a local anchor, the pound sign (#) followed by the value like so:

```
<rdfs:type rdf:resource="#value"/>
```

This pound sign is a reference to the document itself, much like it is used in HTML anchor tags to reference locations within the same document.

Simply the object of "#value" does not qualify as a full URI in SPARQL triples. As the pound sign is a redundant reference, we must also include the absolute path to the file we are querying in the triple. Assuming the page at http://www.example.com/this.rdf, to search on these values, you would need to include the full URI back to the document, along with the value after the pound sign:

```
?Subject ns:predicate <http://www.example.com/this.rdf#value>
```

The complete SPARQL query looks like this:

```
PREFIX mypets: <http://www.shuchow.com/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
SELECT ?name
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description rdfs:type <http://www.shuchow.com/#parrot> .
    ?Description mypets:name ?name
}
ORDER BY ?name
```

Running the query returns these results:

| Name |
|------|
| "Avi" |
| "Dolly" |
| "Hoser" |
| "Moose" |

## Ordering, Limiting, and Offsetting

Note that in this query, we added an ORDER BY clause. SPARQL supports a set of clauses that follow a WHERE clause, which organizes the returned dataset. In addition to ORDER BY, we can use LIMIT and OFFSET clauses.

An ORDER BY clause works very similarly to SQL's ORDER BY clause. This clause sorts the returned dataset by the variable that follows the clause. The results returned are ordered alphabetically if they are strings or ordinal if they are numeric. Ascending and descending options can be specified by using the ASC and DESC functions, respectively.

```
ORDER BY ASC(?name)
ORDER BY DESC(?name)
```

The ascending and descending clauses are optional. If they are left out, the default is ascending order.

SPARQL also supports the LIMIT and OFFSET keywords much like PostgreSQL, MySQL, and other relational database management systems. Both LIMIT and OFFSET are followed by integers. LIMIT will limit the number of results returned to the integer passed to it. OFFSET will shift the start of the returned results to the position of the integer, with the first returned result being position zero.

For example, pets.rdf has 13 animals in the list. If we want to get the 7th and 8th pets, in alphabetical order, we can use LIMIT and OFFSET in conjunction with ORDER BY.

```
PREFIX mypets: <http://www.shuchow.com/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
SELECT ?name
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
  ?Description mypets:name ?name
}
ORDER BY ?name
LIMIT 2
OFFSET 6
```

Note that order matters when you use ORDER BY, LIMIT, or OFFSET. These three clauses must be in that order after the WHERE clause. For example, this will not work:

```
OFFSET 6
ORDER BY ?name
LIMIT 4
```

## UNION and DISTINCT

The UNION keyword joins multiple WHERE groupings together, much like UNION in SQL. The returned results will be a combination of the WHERE groupings. To use a UNION clause, wrap the individual groupings within curly brackets. Join them with the UNION keyword. Place all of this within the regular WHERE curly brackets.

For example, this query will retrieve the names of all parrots and male pets:

```
PREFIX mypets: <http://www.shuchow.com/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT ?name
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
  {
    ?Description rdfs:type <http://www.shuchow.com/#parrot>
    ?Description mypets:name ?name
  }
  UNION
  {
    ?Description mypets:gender "M" .
    ?Description mypets:name ?name
  }
}
ORDER BY ?name
```

| Name |
|------|
| "Avi" |
| "Dolly" |
| "Hoser" |
| "Hoser" |
| "Manfred" |
| "Moose" |
| "Moose" |
| "Snowball" |
| "Wozniak" |

This union does not give us exactly the query we want. Hoser and Moose, male parrots, are in both the first clause and the second. SPARQL supports another SQL keyword, DISTINCT, that will exclude a row based on a column if it has already been included in a previous clause.

Simply add the DISTINCT keyword you wish to ensure uniqueness on, and the results will reflect the change.

```
SELECT DISTINCT ?name
```

| name |
|------|
| "Avi" |
| "Dolly" |
| "Hoser" |
| "Manfred" |
| "Moose" |
| "Snowball" |
| "Wozniak" |

## More SPARQL Features

The queries we will write later will require more complexity, but the features we have discussed are more than we will need for our mashup. SPARQL, however, has many more advanced features including:

- Querying more than one RDF document (if the parser supports it).
- The ability to filter returned results using special operators and a subset of XPATH functions.

The Working Draft document that fully outlines all of SPARQL's features can be found at http://www.w3.org/TR/rdf-sparql-query/. Although it is still in W3C draft stage, many parsers give great support to the language. In future mashups, if you encounter complex RDFs, it would not hurt to be familiar with SPARQL's advanced features to see if it is a viable solution to extract data.

## RDF API for PHP (RAP)

Now we know a bit about RDF and SPARQL, we need a way to actually execute SPARQL queries in an application. There are not any core PHP functions for RDF, but there is a very powerful third party library called RDF API for PHP (RAP). RAP is an open source project, and can do just about anything you require with RDF. RAP is basically a collection of RDF models. Each model suits a specific purpose.

A model named MemModel is a RDF file stored in memory. Another model named DbModel, is used to persist RDF models in a relational database. Each model has specific methods that fit its purpose. DbModel has methods to automatically insert and retrieve the model into and out of a relational database.

All models inherit methods from a generic abstract class called Model. These are generic utility methods that apply to all models. For example, all models need to load a RDF file to do anything with it. The load() method accomplishes this. All models can be represented graphically using the visualize() method, which creates a graphical representation of the RDF file. Version 0.94 includes a method named sparqlQuery() that accepts a SPARQL query and executes it against the model. We will be using this method to create a SPARQL client.

The project home page is located at http://sites.wiwiss.fu-berlin.de/suhl/bizer/rdfapi/. You can download the latest version from there. Documentation is also available, and is very extensive. Download the code, and unzip it. It will create a directory named rdfapi-php. Then, place rdfapi-php in a directory in your application structure. This directory must be accessible by Apache, and in terms of location and permissions.

We will use a few of the previous example SPARQL queries as examples for RAP. In the examples code, the file named rapExample.php executes two SPARQL queries. Let's take a look at this file to see the steps required to use RAP for SPARQL queries.

The file has some preliminary setup PHP code at the top.

```
define("RDFAPI_INCLUDE_DIR", "Absolute/Path/To/rdfapi-php/api/");
require_once(RDFAPI_INCLUDE_DIR . "RdfAPI.php");

//Create SPARQL Client
$sparqlClient = ModelFactory::getDefaultModel();
$sparqlClient->load('http://www.shuchow.com/mashups/ch6/pets.rdf');
```

The very first thing we need to do is create a global variable named RDFAPI_INCLUDE_DIR. The value of this is the absolute path to the rdfapi-php/api directory you just installed. We then use this global variable to include the RdfAPI.php file. These two lines are required for every use of the RAP library.

Next, we create a default model object. The default model is a generic model that all other models inherit from. It is created in the statement that calls getDefaultModel(). The default model object includes the basic methods we will need.

The last line in this block loads the RDF file using the default model's load() method. Here, we load a remote file, but you can also keep a RDF file locally.
