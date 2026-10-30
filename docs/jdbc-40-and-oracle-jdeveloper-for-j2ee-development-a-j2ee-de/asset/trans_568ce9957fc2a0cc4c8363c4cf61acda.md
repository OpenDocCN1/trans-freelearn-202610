
sess.save(catalog);
    sess.save(catalog2);
    tx.commit();
} catch (Exception e) {
    try {
        if (tx != null) {
            tx.rollback();
        }
    } catch (
        org.hibernate.HibernateException excp) {
    }
} finally {
    try {
        if (sess != null) {
            sess.close();
        }
    } catch (
        org.hibernate.HibernateException excp) {
    }
}
}
public void retrieveFromCatalog() {
    try {
        String hqlQuery = "from Catalog";
        config = new Configuration();
        config.addClass(Catalog.class);
        sessionFactory = config.buildSessionFactory();
        sess = sessionFactory.openSession();
        Query query = sess.createQuery(hqlQuery);
        List list = query.list();
        for (int i = 0; i < list.size(); i++) {
            Catalog catalog = (Catalog) list.get(i);
            System.out.println(
                "CatalogId " + catalog.getId() +
                " Journal: " + catalog.getJournal());
            System.out.println(
                "CatalogId " + catalog.getId() +
                " Publisher: " + catalog.getPublisher());
            System.out.println(
                "CatalogId " + catalog.getId() +
                " Edition: " + catalog.getEdition());
            System.out.println(
                "CatalogId " + catalog.getId() + " Title " +
                catalog.getTitle());
System.out.println(
    "CatalogId " + catalog.getId() +
    " Author: " + catalog.getAuthor());
}
if (sess != null) {
    sess.close();
}
} catch (org.hibernate.HibernateException e) {
}
}
public void updateCatalog() {
    try {
        String hqlQuery = "from Catalog";
        config = new Configuration();
        config.addClass(Catalog.class);
        sessionFactory = config.buildSessionFactory();
        sess = sessionFactory.openSession();
        Query query = sess.createQuery(hqlQuery);
        List list = query.list();
        Catalog catalog = (Catalog) list.get(0);
        catalog.setPublisher("Oracle Magazine");
        tx = sess.beginTransaction();
        sess.saveOrUpdate(catalog);
        tx.commit();
    } catch (Exception e) {
        try {
            if (tx != null) {
                tx.rollback();
            }
        } catch (
            org.hibernate.HibernateException excp) {
        }
    } finally {
        try {
            if (sess != null) {
                sess.close();
            }
        } catch (
            org.hibernate.HibernateException excp) {
        }
    }
}
    public void deleteFromCatalog() {
        try {
String hqlQuery = "from Catalog as catalog WHERE
    catalog.edition='March-April 2005'";
config = new Configuration();
config.addClass(Catalog.class);
sessionFactory = config.buildSessionFactory();
sess = sessionFactory.openSession();
Query query = sess.createQuery(hqlQuery);
List list = query.list();
Catalog catalog = (Catalog) list.get(0);
tx = sess.beginTransaction();
sess.delete(catalog);
tx.commit();
} catch (Exception e) {
    try {
        if (tx != null) {
            tx.rollback();
        }
    } catch (
        org.hibernate.HibernateException excp) {
    }
} finally {
    try {
        if (sess != null) {
            sess.close();
        }
    } catch (
        org.hibernate.HibernateException excp) {
    }
}
}
public static void main(String[] argv) {
    HibernateDB hibernateDB = new HibernateDB();
    hibernateDB.addToCatalog();
    /*hibernateDB.retrieveFromCatalog();
    hibernateDB.updateCatalog();
    hibernateDB.deleteFromCatalog();*/
}
}

将 `HibernateDB.java` 复制到 Hibernate 项目中。要向 `Catalog` 表添加数据，请注释掉除 `addToCatalog()` 之外的所有方法。要运行 Java 应用程序，请右键单击应用程序节点并选择 **运行**。要从数据库检索数据，请注释掉 `HibernateDB.java` 类中除 `retrieveFromCatalog()` 之外的所有方法并运行应用程序。Hibernate 应用程序检索 `Catalog` 表数据。

```
```
CatalogId 1 Journal: Oracle Magazine
CatalogId 1 Publisher: Oracle Publishing
CatalogId 1 Edition: Jan-Feb 2004
CatalogId 1 Title Understanding Optimization
CatalogId 1 Author: Kimberly Floss ..
CatalogId 2 Journal: Oracle Magazine
CatalogId 2 Publisher: Oracle Publishing
CatalogId 2 Edition: March-April 2005
CatalogId 2 Title Starting with Oracle ADF
CatalogId 2 Author: Steve Muench
```
```

要更新数据库表，请注释掉除 `updateCatalog()` 之外的所有方法并运行 `HibernateDB` 应用程序。要从 `Catalog` 表中删除，请注释掉除 `deleteFromCatalog()` 方法之外的所有方法并运行 `HibernateDB` 应用程序。

## 摘要

无数据库持久化和查询服务的对象/关系映射需要 JDBC API 和特定于厂商的 SQL 脚本来创建、更新、删除数据库表。使用 Hibernate 时不需要 JDBC API 和 SQL 脚本。Hibernate 生成用于向数据库添加、检索、更新和删除数据的 SQL 脚本。Hibernate 提供了 `net.sf.hibernate.tool.hbm2java.Hbm2JavaTask` Apache Ant 构建任务，用于从 `hbm.xml` 配置文件生成 JavaBean 类，以及 `org.hibernate.tool.hbm2ddl.SchemaExportTask` Apache Ant 构建任务，用于从 `hbm.xml` 配置文件生成数据库表。

## 索引

### A

ADF 业务组件
关于 369
BC4J 项目，配置 371, 372
BC4J Web 应用程序，开发 390-394
业务组件，创建 372-390
实体对象 369
环境，设置 370
JDBC 4.0 版本 395
视图对象 369
Apache POI。另请参阅 Apache POI HSSF
JDBC 4.0 功能，添加 365-368
JDBC 4.0 版本 364-368
Apache POI HSSF
关于 355
Apache POI 库，下载 355
类 360
环境，设置 355-357
Excel 电子表格，创建 359, 360
HSSFCell 360
HSSFCellStyle 360
HSSFChart 360
HSSFDateUtil 360
HSSFFont 360
HSSFHeader 360
HSSFPrintSetup 360
HSSFRow 360
HSSFSheet 360
HSSFWorkbook 360
实现 355
Application Development Framework 业务组件。另请参阅 ADF 业务组件

### B

BC4J 370
BC4J 项目，配置 371, 372
BC4J Web 应用程序，开发 390-394
业务组件，创建 372-390
Business Components for Java。另请参阅 BC4J

### C

CMP 116
连接缓存属性
connectionCacheName 67
connectionCachingEnabled 67
fastConnectionFailoverEnabled 67
连接工厂
连接 URL 58
工厂类 58
登录超时 58
密码 58
用户名 58
连接接口
close() 方法 9
commit() 方法 8
createArrayOf() 方法 9
createBlob() 方法 9
createClob() 方法 9
createNClob() 方法 9
createSQLXML() 方法 9
createStruct() 方法 9
getClientInfo() 方法 9
isValid() 方法 9
方法 9
setAutoCommit(boolean autoCommit) 方法 8
setClientInfo() 方法 9
setReadOnly() 方法 8
连接池
关于 34
缺点 34
连接池元素
连接工厂，子元素 65, 66
连接池属性
关于 49
属性 56
initialPoolSize 50
maxIdleTime 50
maxPoolSize 50
maxStatements 50
minPoolSize 50
propertyCycle 50
属性集 A 56
属性集 B 57
Container-Managed-Persistence。另请参阅 CMP

### D

数据，使用 Hibernate API 删除 416-421
数据库
从 Web 应用程序连接 67
使用 JSTL SQL 标签库连接 79
数据库连接，使用 JDBC 4.0 驱动程序 40-42
DatabaseMetadata 接口
关于 18
autoCommitFailureClosesAllResultSets() 方法 21
getClientInfoProperties() 方法 21
getColumns(String catalog,String schemaPattern,String tableNamePattern,String columnNamePattern) 方法 20
getFunctionColumns() 方法 21
getFunctions() 方法 21
getMaxColumnsInSelect() 方法 19
getProcedures(String catalog,String schemaPattern, String procedureNamePattern) 方法 20
getRowIdLifetime() 方法 20
getSchemas() 方法 21
getTables(String catalog,String schemaPattern,String tableNamePattern,String[] types) 方法 19
getTypeInfo() 方法 18
supportsBatchUpdates() 方法 18
supportsGroupBy() 方法 19
supportsResultSetConcurrency() 方法 18
supportsResultSetType() 方法 18
supportsSavepoints() 方法 18
supportsStoredFunctionsUsingCallSyntax() 方法 21
supportsTransactions() 方法 18
数据库表。另请参阅 Oracle 数据库；
创建 87-94
创建，使用 Hibernate 405-412
创建，使用 JSTL SQL 1.1 标签 87
数据，使用 Hibernate API 删除 416-421
数据添加，使用 Hibernate 413, 415
数据检索，使用 Hibernate 415
修改，使用 Web RowSet 245, 247
查询 98-103
查询，使用 JSTL SQL 1.1 标签 98
行，删除 197
更新 94-97
更新，从 XML 文档 196, 197
更新，使用 JSTL SQL 1.1 标签 94
更新，使用 Hibernate Java 应用程序 415
更新，使用 Web RowSet 260
数据源 125
DataSource 接口
关于 45
优点 47
基本实现 46
连接池实现 47
分布式事务实现 47
实现，类型 46
DB2 数据库
JBoss 服务器，配置 121, 122
DriverManager 类
Class.forName() 方法 7
getConnection() 方法 8
getDrivers() 方法 8
