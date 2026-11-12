
SolrQuery represents a query object, which contains the field information based on which the search has to be performed, the fields to return, and so on. The query method of SolrServer sends the search request to Solr search server using Apache Commons HttpClient and returns a QueryResponse object from which you can extract the Solr documents that matched the search query.

- search(String) method that only returns Solr document(s) corresponding to FlightDescription entity in Solr search server:

```
public static QueryResponse FlightDescription.search(String queryString) {
 String searchString = 
 "FlightDescription_solrsummary_t:" + queryString;
 return search(new SolrQuery(searchString.toLowerCase()));
}
```

In the given code, the SolrQuery object is created using the searchString. The searchString specifies the Solr query used for finding matching Solr documents. As searchString already contains the constant value "FlightDescription_solrsummary_t:", which means that you can only search for Solr documents that contain "FlightDescription_solrsummary_t" field. If you remember from the earlier discussion, the "FlightDescription_solrsummary_t" field is only available in Solr documents which have been added corresponding to the FlightDescription entity.

Let's now look at how the FlightDescriptionSearchController controller makes use of search methods defined in the FlightDescription JPA entity to search documents indexed by Solr search server.

### Searching Solr documents

FlightDescriptionSearchController defines methods which search for Solr documents corresponding to the FlightDescription entity. The following code listing shows FlightDescriptionSearchController class:

```
@Controller
public class FlightDescriptionSearchController {
 private List<List<MySolrField>> getAllFields() {
 QueryResponse response = FlightDescription.search("*");
 SolrDocumentList documentList = response.getResults();
 return getSolrDocumentFieldList(documentList);
 }

 private List<List<MySolrField>> getMatchingFields() {
 SolrQuery solrQuery = new SolrQuery().
 setQuery("flightdescription_solrsummary_t:*").
 setParam("fl", "flightdescription.origin_s");
 QueryResponse response =
 FlightDescription.search(solrQuery);
 SolrDocumentList documentList = response.getResults();
 return getSolrDocumentFieldList(documentList);
 }

 private List<List<MySolrField>>
 getSolrDocumentFieldList(SolrDocumentList list) {
 List<List<MySolrField>> matchingDocList
 = new ArrayList<List<MySolrField>>();
 ...
 return matchingDocList;
 }
}
```

The getAllFields method invokes search(String queryString) method of FlightDescription entity and passes * as the method argument. As we saw earlier, the search(String queryString) method of FlightDescription will create the following query: "FlightDescription_solrsummary_t:*", which means search for all Solr documents, which contain "FlightDescription_solrsummary_t" field. This query will return all the Solr documents corresponding to FlightDescription entity that we added to Solr index.

The getMatchingFields method invokes the search(SolrQuery query) method passing the SolrQuery object, which queries for all Solr documents corresponding to FlightDescription JPA entity but specifies that the query result should only contain the flightdescription.origin_s field. The setQuery parameter of SolrQuery specifies the query and setParam specifies that only flightdescription.origin_s field should be returned in the result.

The getResults method of the QueryResponse object returns SolrDocumentList representing the list of matching Solr documents returned by the query.

The getSolrDocumentFieldList method takes SolrDocumentList as the argument and extracts SolrDocument instances from it. The method then extracts field names and their values from each SolrDocument instance to create a List<List<MySolrField>>. The MySolrField represents a custom class that we created in flightapp-web project to represent a single field-value pair in SolrDocument.

The /WEB-INF/views/flightdescriptionsearch/index.jsp JSP page displays data returned by getFields and getMatchingFields methods. This is the reason why selecting Flight Description Search Controller View menu option shows two different types of tables. One table type shows all the Solr document fields and the other table type only shows the flightdescription.origin_s field.

### There's more...

Solr index is updated in @PreRemove, @PostPersist, and the @PostUpdate annotated method. So, what if the transaction fails to commit but the entity data is stored as Solr document in Solr search server? You need to take care of maintaining the integrity yourself, because Roo doesn't help you there.

Let's now look at the attributes that @RooSolrSearchable defines to customize names of Roo-generated methods in *_Roo_SolrSearch.aj AspectJ ITD.

### Customizing Roo-generated *_Roo_SolrSearch.aj AspectJ ITD

The following table describes the attributes of @RooSolrSearchable annotation:

| Attribute | Description |
|---|---|
| deleteIndexMethod | Specifies a custom name for the `deleteIndex` method. Value blank "" instructs Roo not to generate `deleteIndex` method. |
| indexMethod | Specifies a custom name for the `index` methods. Value blank "" instructs Roo not to generate `index` methods. |
| postPersistOrUpdateMethod | Specifies a custom name for the `postPersistOrUpdate` methods. Value blank "" instructs Roo not to generate `postPersistOrUpdate` methods. |
| preRemoveMethod | Specifies a custom name for the `preRemove` method. Value blank "" instructs Roo not to generate `preRemove` method. |
| searchMethod | Specifies a custom name for the `search` method, which accepts `SolrQuery` as argument. Value blank "" instructs Roo not to generate `search` method, which accepts `SolrQuery` argument. |
| simpleSearchMethod | Specifies a custom name for the `search` method, which accepts `String` as argument. Value blank "" instructs Roo not to generate `search` method, which accepts `String` argument. |

## Developing Add-ons and Removing Roo from Projects

In this chapter, we will cover:

- Setting up GnuPG for add-on development
- Installing an installable add-on
- Developing a simple add-on
- Developing an advanced add-on
- Converting non-OSGi JDBC drivers into OSGi-compliant bundles
- Removing Roo with push-in refactoring
- Adding Roo to a project using pull-out refactoring
- Upgrading to the latest version of Roo

### Introduction

In previous chapters, we saw that executing a Roo command kicks-off code generation logic in Roo add-ons. You may want to create a custom add-on for functionality that is not yet supported by base or installable add-ons. For instance, currently there is no Roo add-on that provides support for creating Java portlets using the Spring Portlet MVC. So, we can develop a Roo add-on which is responsible for scaffolding Spring Portlet MVC controllers and JSPs from JPA entities.

In this chapter, we'll look at recipes which show how Roo simplifies installing, developing, and publishing add-ons. Also, we'll take a broad look at the Roo architecture and Roo commands which you'll find useful while developing and testing add-ons.

### Setting up GnuPG for add-on development

Roo makes use of GnuPG (http://www.gnupg.org/) to sign add-ons. Signing of add-ons ensures that Roo users download and install only trusted add-ons. The add-on creator module of Roo signs the add-on with his secret PGP key and publishes the public PGP key to a public key server. A Roo user needs to tell Roo explicitly that it trusts an add-on by adding a public PGP key to Roo's key store. This allows Roo to download and install the add-on. If the public PGP key is not added to Roo's key store, the add-on will not be downloaded and installed.

In this recipe, we'll look at how to install GnuPG, create a key-pair (consisting of secret and public keys), and publish the public key to a public key server.

#### Getting ready

If you are using Windows or UNIX, download and install GnuPG from the following location: http://www.gnupg.org/download/. If you are using Mac, download and install GnuPG for Mac from the following location: http://macgpg.sourceforge.net/.

Installing GnuPG on Windows will create the following installation directory: C:\\Program Files\\GNU\\GnuPG.

#### How to do it...

The following steps will demonstrate how to set up GnuPG:

1. Open the command prompt and execute the following GnuPG `list-secret-keys` command to view secret PGP keys that you may have created earlier:

```
C:\\Users\\Ashish>gpg --list-secret-keys
gpg: keyring 'C:/Users/Ashish/AppData/Roaming/gnupg\\secring.gpg'
created
```

The output shows that a `secring.gpg` file is created if it is not found. The `secring.gpg` file contains key information.

2. Create a new key-pair consisting of a secret PGP key and public PGP key using GnuPG's `gen-key` command, as shown here:

```
C:\\Users\\Ashish>gpg --gen-key
...
Please select what kind of key you want:
```
