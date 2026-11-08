# DefaultSessionProvider

DefaultSessionProvider requires no constructor parameters, and uses the following API call to generate SessionFactory from Hibernate: sessionFactory = new Configuration().configure().buildSessionFactory();

The created sessionFactory instance is used to create new sessions, which HQLDataFactory uses to query Hibernate.

HQLDataFactory provides two constructors. The first constructor takes in SessionProvider, as described earlier. The second constructor simply takes in a Hibernate Session instance, which it uses to query Hibernate. This constructor uses StaticSessionProvider, under the covers, to pass the Session into the HQLDataFactory.

Once you've instantiated your factory, you may add named queries to the factory by making the following API call:

```
void setQuery(String name, String queryString);
```

The setQuery method takes in the name of the query, and the Hibernate query, in order to execute.

HQLDataFactory uses Hibernate's query language, which is well-documented at http://www.hibernate.org/hib_docs/reference/en/html/queryhql.html. You may include report parameters in your query by using the HQL syntax :ParameterName. The max results and query timeout parameters are supported by HQLDataFactory.