
+   9. Two lines below the line from the previous step, you should already have the following import statement:
+   ```
+   from django.core.management import execute_from_command_line
+   ```
+   Change this to the following:
+   ```
+   from configurations.management import execute_from_command_line
+   ```
+   This will make the manage.py script use the Django configuration’s execute_from_command_line function, instead of the Django built-in one.
+   Save manage.py.

+   10. Start the Django dev server. If it begins without error, you can be confident that the changes you made have worked. To be sure, check that the pages load in your browser. Open http://127.0.0.1:8000/ and try browsing around the site. Everything should look and feel as it did before:

![](img/4840a43efd335abd10b93708225692d6_654_0.png)

In this exercise, we installed django-configurations and refactored our settings.py file to use its Configuration class to define our settings. We added the Dev and Prod configurations and made DEBUG, ALLOWED_HOSTS, and SECRET_KEY settable with environment variables. Finally, we updated manage.py to use the Django configuration’s execute_from_command_line function, which enabled the use of this new settings.py format.

In the next section, we will cover dj-database-url, a package that makes it possible to configure your Django database settings using URLs.

The output is as follows:

```
Collecting dj-database-url
    Downloading https://files.pythonhosted.org/packages/d4/a6/4b8578c1848690d0c307c7c0596af2077536c9ef2a04d42b00fabaa7e49d/dj_database_url-0.5.0-py2.py3-none-any.whl
Installing collected packages: dj-database-url
Successfully installed dj-database-url-0.5.0
```

Now, dj_database_url can be imported into settings.py, and the dj_database_url.parse method can be used to transform the URL into a dictionary that Django can use. We can use its return value to set the default (or other) item in the DATABASES dictionary:

```
import dj_database_url

DATABASES = {'default': dj_database_url.parse(
'postgres://bookr:b00ks@db.example.com:5432/bookr_django'
)}
```

Alternatively, for our SQLite database, we can utilize the BASE_DIR setting, as we are already, and include it in the URL:

```
import dj_database_url

DATABASES = {'default': dj_database_url.parse(
    f'sqlite:///{BASE_DIR}/db.sqlite3')}
```

After parsing, the DATABASES dictionary is similar to what we defined before. It includes some redundant items that do not apply to an SQLite database (USER, PASSWORD, HOST, and so on), but Django will ignore them:

```
DATABASES = {
    'default': {
        'NAME': '/Users/ben/bookr/bookr/db.sqlite3',
        'USER': '',
        'PASSWORD': '',
        'HOST': '',
        'PORT': '',
        'CONN_MAX_AGE': 0,
        'ENGINE': 'django.db.backends.sqlite3'
    }
}
```

This method of setting the database connection information is not that useful, since we are still statically defining the data in settings.py. The only difference is that we are using a URL instead

of a dictionary. `dj-database-url` can also automatically read the URL from an environment variable. This will allow us to override these values by setting them in the environment.

To read the data from the environment, use the `dj_database_url.config` function, like this:

```
```python
import dj_database_url

DATABASES = {'default': dj_database_url.config()}
```
```

The URL is automatically read from the `DATABASE_URL` environment variable.

We can improve on this by also providing a `default` argument to the config function. This is the URL that will be used by default if one is not specified in an environment variable:

```
```python
import dj_database_url

DATABASES = {
    'default': dj_database_url.config(
        default=f'sqlite:///{BASE_DIR}/db.sqlite3')}
```
```

This way, we can specify a default URL that can be overridden by an environment variable in production.

We can also specify the environment variable that the URL is read from by passing in the `env` argument – this is the first positional argument. In this way, you could read multiple URLs for different database settings:

```
```python
import dj_database_url

DATABASES = {
    'default': dj_database_url.config(
        default=f'sqlite:///{BASE_DIR}/db.sqlite3'),
    'secondary': dj_database_url.config(
        'DATABASE_URL_SECONDARY',
        default=f'sqlite:///{BASE_DIR}/db-secondary.sqlite3')
}
```
```

In this example, the `default` item’s URL is read from the `DATABASE_URL` environment variable, and `secondary` is read from `DATABASE_URL_SECONDARY`.

django-configurations also provides a config class that works in tandem with dj_database_url: DatabaseURLValue. This differs slightly from dj_database_url.config in that it generates the entire DATABASES dictionary, including the default item. For example, consider the following code:

```
import dj_database_url

DATABASES = {'default': dj_database_url.config()}
```

This code is equivalent to the following:

```
from configurations import values

DATABASES = values.DatabaseURLValue()
```

Do not write DATABASES['default'] = values.DatabaseURLValue(), as your dictionary will be doubly nested.

If you need to specify multiple databases, you will need to fall back to dj_database_url.config directly, rather than using DatabaseURLValue.

Like other values classes, DatabaseURLValue takes a default value as its first argument. You might also want to use the environ_prefix argument and set it to DJANGO so that the environment variable being read is consistently named with the others. A full example of using DatabaseURLValue would, therefore, be like this:

```
DATABASES = values.DatabaseURLValue(
    f'sqlite:///{BASE_DIR}/db.sqlite3',
    environ_prefix='DJANGO')
```

By setting the environ_prefix like this, we can set the database URL using the DJANGO_DATABASE_URL environment variable (rather than just DATABASE_URL). This means it is consistent with other environment variable settings that also start with DJANGO_, such as DJANGO_DEBUG or DJANGO_ALLOWED_HOSTS.

Note that even though we are not importing dj-database-url in settings.py, django-configurations uses it internally, so it still must be installed.

In the next exercise, we will configure Bookr to use DatabaseURLValue to set its database configuration. It will be able to read from an environment variable and fall back to a default we specify.

## Exercise 15.02 – dj-database-url and setup

In this exercise, we will install dj-database-url using pip3. Then, we will update Bookr’s settings.py to configure the DATABASE setting using a URL, which is read from an environment variable. Let’s get started with the steps:

+   1. In a terminal, make sure you have activated the bookr virtual environment, and then run the following command to install dj-database-url using pip3:

```
pip3 install dj-database-url
```

The installation process will run, and you should have output similar to this:

```
((bookr) → bookr pip install dj-database-url
Collecting dj-database-url
Using cached dj_database_url-1.2.0-py3-none-any.whl (7.1 kB)
Requirement already satisfied: dj-database-url==3.2 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from dj-database-url) (4.0)
Requirement already satisfied: backports.zoneinfo; python_version < "3.9" in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from Django>=3.2->dj-database-url) (0.2.1)
Requirement already satisfied: asgiref>=3.4.1 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from Django>=3.2->dj-database-url) (3.4.1)
Requirement already satisfied: sqlparse>=0.2.2 in /Users/ben/.virtualenvs/bookr/lib/python3.8/site-packages (from Django>=3.2->dj-database-url) (0.4.2)
Installing collected packages: dj-database-url
Successfully installed dj-database-url-1.2.0
```

Figure 15.5 – dj-database-url installation with pip

+   2. In PyCharm, open settings.py in the bookr package directory. Scroll down to find where the DATABASES attribute is being defined. Replace it with the values.DatabaseURLValue class. The first argument (default value) should be the URL to the SQLite database: f'sqlite:///{BASE_DIR}/db.sqlite3'. Also, pass in environ_prefix, set to DJANGO. After completing this step, you should be setting the attribute like this:

```
DATABASES = values.DatabaseURLValue(
    f'sqlite:///{BASE_DIR}/db.sqlite3',
    environ_prefix='DJANGO'
)
```

Save settings.py.

+   3. Start the Django dev server. As with Exercise 15.01 – Django configurations setup, if it starts fine, you can be confident that your change was successful. To be sure, open http://127.0.0.1:8000/ in a browser and check that everything looks and behaves as it did before. You should visit a page that queries from the database (such as the Books List page) and check that a list of books is displayed:
