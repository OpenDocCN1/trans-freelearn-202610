

@RooSolrSearchable
public class FlightDescription {
...
}
```

给定代码显示，@RooSolrSearchable 注解已添加到 FlightDescription 实体。如果你查看 Flight 实体，你会发现 @RooSolrSearchable 注解也已添加到它。

- 创建一个 *_Roo_SolrSearch.aj AspectJ ITD 文件（对应于项目中的每个 JPA 实体。_*_Roo_SolrSearch.aj）将方法引入 JPA 实体类，用于从 Solr 索引中添加、更新和删除实体。另外，_*_Roo_SolrSearch.aj 定义用于使用 SolrJ 客户端库查询 Solr 服务器的方法。

## *_Roo_SolrSearch.aj AspectJ ITD 引入的方法和属性

让我们现在来看看 FlightDescription_Roo_SolrSearch.aj 文件引入的方法和属性：

- solrServer 属性指向在 applicationContext.xml 文件中配置的 CommonsHttpSolrServer bean，如下所示：

```java
@Autowired
transient SolrServer FlightDescription.solrServer;
```

- solrServer()：一个静态方法，返回 ITD 文件引入的 solrServer 属性，如下所示：

```java
public static final SolrServer FlightDescription.solrServer() {
    SolrServer _solrServer = new FlightDescription().solrServer;
    ..
    return _solrServer;
}
```

- indexFlightDescriptions：一个静态方法，将 FlightDescription 实体实例的集合添加到 Solr 索引中，如下所示：

```java
import org.springframework.scheduling.annotation.Async;
...
@Async
public static void FlightDescription.indexFlightDescriptions
(Collection<FlightDescription> flightdescriptions) {
    java.util.List<SolrInputDocument> documents =
    new java.util.ArrayList<SolrInputDocument>();
    
    for (FlightDescription flightdescription :
    flightdescriptions) {
        SolrInputDocument sid = new SolrInputDocument();
        sid.addField("id", "flightdescription_" +
                flightdescription.getId());
        sid.addField("flightdescription.id_l",
                flightdescription.getId());
        sid.addField("flightdescription.origin_s",
                flightdescription.getOrigin());
        ...
        sid.addField("flightdescription.price_f",
                flightdescription.getPrice());
        sid.addField("flightdescription_solrsummary_t", ...);
        documents.add(sid);
    }
    try {
        SolrServer solrServer = solrServer();
        solrServer.add(documents);
        solrServer.commit();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

给定代码显示，indexFlightDescriptions 方法标注了 Spring 的 @Async 注解，这意味着它是异步调用的。该方法遍历所有 FlightDescription 实例（作为方法参数传递）并创建一个 SolrInputDocument 列表。SolrJ 的 SolrInputDocument 类表示你想要馈送到 Solr 服务器以进行索引的文档。SolrInputDocument 的 addField 方法标识你想要添加到文档的字段。

Roo 添加到 SolrInputDocument 的字段名具有以下命名约定：
<entity-simple-name>.<field-name>_<field-type>

这里，entity-simple-name 是 JPA 实体的简单名称，field-name 是字段的名称，field-type 是字段的类型。因此，origin 字段以 flightdescription.origin_s 的名称添加到 SolrInputDocument，price 字段以 flightdescription.price_f 的名称添加。

如果 JPA 实体字段类型不是 Integer、String、Long、Boolean、Float、Double 或 Date，则 JPA 实体字段添加到 SolrInputDocument 时的字段名如下所示：
<entity-simple-name>.<field-name>_t

例如，`flightapp-web` 项目中的 `Flight` 类包含类型为 `FlightDescription` 的 `flightDescription` 关系字段，该字段以 `flight.flightdescription_t` 的名称添加到 `SolrInputDocument`（参考 `Flight_Roo_SolrSearch.aj` AspectJ ITD 文件）。

你可能会想知道，为什么 Roo 不使用确切名称将 JPA 实体字段添加到 `SolrInputDocument` 中。以下是 Solr 工作原理的简要说明：

`SolrInputDocument` 表示你要添加到 Solr 搜索服务器的文档。该文档由字段组成，你需要告诉 Solr 搜索服务器，其中哪些字段应该被 **索引**。需要注意的是，如果某个字段 *未* 被索引，则你无法基于该字段搜索或排序文档。你通过在 `SOLR_HOME\example\solr\conf` 目录中的 `schema.xml` 文件中指定字段，告诉 Solr 搜索服务器文档的哪些字段应该被索引。Solr 具有动态字段的概念，即如果字段遵循标准命名约定，则它会被 Solr 搜索服务器自动索引。`schema.xml` 文件中的以下 XML 片段定义了将由 Solr 自动索引的动态字段：

```xml
<dynamicField name="*_s" type="string" indexed="true" stored="true"/>
<dynamicField name="*_l" type="long" indexed="true" stored="true"/>
<dynamicField name="*_t" type="text" indexed="true" stored="true"/>
<dynamicField name="*_f" type="float" indexed="true" stored="true"/>
```

给定的 XML 片段指示 Solr 索引与模式 `*_s`、`*_l`、`*_t` 或 `*_f` 匹配的任何字段。因此，现在你可以看到 Roo 生成的字段名与 Solr 定义的动态字段之间的联系。

indexFlightDescriptions 方法还向 `SolrInputDocument` 添加了一个 `id` 字段名。任何 SolrInputDocument 必须包含一个名为 id 的字段，该字段在 Solr 索引中 *唯一* 标识文档。默认情况下，Roo 将 id 字段的值设置为 "flightdescription_" + flightdescription.getId()。我们将在本节后面看到，此 id 字段值用于从 Solr 索引中删除文档。

indexFlightDescriptions 方法还在 SolrInputDocument 中添加了一个额外字段 flightdescription_solrsummary_t，以便可以用于搜索 Solr 为 FlightDescription JPA 实体索引的所有文档。同样，Flight_Roo_SolrSearch.aj AspectJ ITD 的 indexFlightDescriptions 方法在 SolrInputDocument 中添加 flight_solrsummary_t 字段，以允许搜索 Solr 为 Flight JPA 实体索引的文档。

`indexFlightDescriptions` 方法中的以下代码将 `SolrInputDocument`s 添加到 Solr 索引：

```java
SolrServer solrServer = solrServer();
solrServer.add(documents);
solrServer.commit();
```

- indexFlightDescription：一个静态方法，将 FlightDescription 实体实例添加到 Solr 索引，如下所示：

```java
public static void
FlightDescription.indexFlightDescription(FlightDescription
flightdescription) {
List<FlightDescription> flightdescriptions =
new ArrayList<FlightDescription>();
flightdescriptions.add(flightdescription);
indexFlightDescriptions(flightdescriptions);
}
```

如给定代码所示，indexFlightDescription 方法将将 FlightDescription 实例添加到 Solr 索引的责任委托给 indexFlightDescriptions 方法。

- deleteIndex：一个静态方法，删除与 FlightDescription JPA 实体实例对应的 Solr 文档，如下所示：

```java
@Async
public static void
FlightDescription.deleteIndex(FlightDescription
flightdescription) {
SolrServer solrServer = solrServer();
try {
solrServer.deleteById("flightdescription_" +
flightdescription.getId());
solrServer.commit();
} catch (Exception e) {
e.printStackTrace();
}
}
```

在给定代码中，SolrServer 的 deleteById 方法删除（从 Solr 索引中）id 属性值为 "flightdescription_" + flightdescription.getId() 的文档。Spring 的 @Async 注解意味着 deleteIndex 方法被异步调用。

- postPersistOrUpdate 方法，当 FlightDescription JPA 实体实例在数据库中被持久化或更新时调用。该方法负责使用修改后的 JPA 实体实例数据添加或更新 Solr 索引，如下所示：

```java
import javax.persistence.PostPersist;
import javax.persistence.PostUpdate;
...
@PostUpdate
@PostPersist
private void FlightDescription.postPersistOrUpdate() {
    indexFlightDescription(this);
}
```

@PostUpdate 和 @PostPersist JPA 注解表明，当 FlightDescription JPA 实体在数据库中被更新或持久化时，将调用 postPersistOrUpdate 方法。对 indexFlightDescription 方法的调用表明，实体数据已更新或添加到 Solr 索引。

- preRemove 方法，通过调用 deleteIndex 方法从 Solr 索引中删除实体数据：

```java
import javax.persistence.PreRemove;
...
@PreRemove
private void FlightDescription.preRemove() {
    deleteIndex(this);
}
```

@PreRemove JPA 注解意味着，preRemove 方法在 JPA 实体实例从数据库中删除之前被调用。

- search(SolrQuery query) 方法，允许搜索与搜索查询匹配的 Solr 文档：

```java
public static QueryResponse FlightDescription.search(SolrQuery
query)
    {
        try {
            return solrServer().query(query);
        } catch (Exception e) {
            e.printStackTrace();
        }
    return new QueryResponse();
}
```

