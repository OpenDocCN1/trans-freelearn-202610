# DatabaseTemplate and ORMs

+   - `DatabaseTemplate` focuses on accessing the database without writing lots of boiler plate code
+   - ORMs focus on mapping tables to objects

`DatabaseTemplate` does not contest with ORM. The choice we must make is between using SQL and processing result sets or using an ORM.

> Before going into detail about ORMs and DatabaseTemplate, it may be useful to look at a quick example of a popular Python ORM: **SQLAlchemy** (http://www.sqlalchemy.org). We could have picked any number of ORMs for this demonstration.

```
from sqlalchemy import *

engine = create_engine("sqlite:///tmp/springpython.db", echo=True)
metadata = BoundMetaData(engine)
article_table = Table('Article', metadata,
    Column('id', Integer, primary_key=True),
    Column('title', String()),
    Column('wiki_text', String()))

article_mapper = mapper(Article, article_table)

session = create_session(bind_to=engine)
articles = session.query(Article)
```

This demonstrates how we would use SQLAlchemy to define the mapping between the ARTICLE table and the Article class. ORMs also offer many other query options, including filters and, critieria. The key purpose of ORMs is to map databases to objects. There is boiler plate with using ORMs just as there is with raw SQL.

# Solutions provided by DatabaseTemplate

If we choose `DatabaseTemplate` for our data needs, we would write our updates, inserts, deletes, and queries using pure SQL. If our team was comprised of database designers and software developers who are all familiar with SQL, this would be of huge benefit—being a more natural fit to their skills. The whole team could contribute to the effort of designing tables, queries, and data management by speaking the common language of SQL.