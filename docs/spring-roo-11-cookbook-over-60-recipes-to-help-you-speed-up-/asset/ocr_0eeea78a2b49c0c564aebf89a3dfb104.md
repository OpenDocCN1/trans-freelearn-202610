# There's more...

Solr index is updated in @PreRemove, @PostPersist, and the @PostUpdate annotated method. So, what if the transaction fails to commit but the entity data is stored as Solr document in Solr search server? You need to take care of maintaining the integrity yourself, because Roo doesn't help you there.

Let's now look at the attributes that @RooSolrSearchable defines to customize names of Roo-generated methods in *_Roo_SolrSearch.aj AspectJ ITD.

# Customizing Roo-generated *_Roo_SolrSearch.aj AspectJ ITD

The following table describes the attributes of @RooSolrSearchable annotation:

| Attribute | Description |
|---|---|
| deleteIndexMethod | Specifies a custom name for the `deleteIndex` method. Value blank "" instructs Roo not to generate `deleteIndex` method. |
| indexMethod | Specifies a custom name for the `index` methods. Value blank "" instructs Roo not to generate `index` methods. |
| postPersistOrUpdateMethod | Specifies a custom name for the `postPersistOrUpdate` methods. Value blank "" instructs Roo not to generate `postPersistOrUpdate` methods. |
| preRemoveMethod | Specifies a custom name for the `preRemove` method. Value blank "" instructs Roo not to generate `preRemove` method. |
| searchMethod | Specifies a custom name for the `search` method, which accepts `SolrQuery` as argument. Value blank "" instructs Roo not to generate `search` method, which accepts `SolrQuery` argument. |
| simpleSearchMethod | Specifies a custom name for the `search` method, which accepts `String` as argument. Value blank "" instructs Roo not to generate `search` method, which accepts `String` argument. |