# Mapping queries by convention over configuration

Our class definition happens to have the same property names as the columns in our database. Spring Python offers `SimpleRowMapper` as a convenient out-of-the-box mapper that takes advantage of this.

Instead of writing the specialized `ArticleMapper`, let's use Spring Python's `SimpleRowMapper` instead.

```
```python
return dt.query("SELECT id, title, wiki_text FROM ARTICLE",
    SimpleRowMapper(Article))
```
```

`SimpleRowMapper` requires that the class has a default constructor, and also that the class's properties match the query's.

![](bbox=[0.2, 0.4, 0.26, 0.43])

> It's important to remember that column-to-property matching is based on the query, not the table. This means we can use SQL aliasing to link up table columns with objects.

# Mapping queries into dictionaries

Spring Python also offers the `DictionaryRowMapper`, which conveniently maps the query into a Python dictionary.

Instead of using the `SimpleRowMapper`, let's use Spring Python's `DictionaryRowMapper` instead.

```
```python
return dt.query("SELECT id, title, wiki_text FROM ARTICLE",
    DictionaryRowMapper())
```
```

![](bbox=[0.2, 0.64, 0.26, 0.67])

> This last step breaks out of our original requirement to return a list of `Article` objects. But it is a convenient way of providing a simple 'window on data' scenario and may perfectly match our needs.