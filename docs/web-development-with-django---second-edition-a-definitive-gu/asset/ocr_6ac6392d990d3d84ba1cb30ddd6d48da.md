The NAME key defines the name of the database, which can have any value. But for SQLite3, since the database is created as a file, NAME can have the full path of the directory where the file needs to be created. The full path of the db file is processed by joining (or concatenating) the previously defined path in BASE_DIR with db.sqlite3. Note that BASE_DIR is the project directory as already defined in the settings.py file.

If you are using other databases, such as PostgreSQL, MySQL, and so on, changes will have to be made in the preceding database settings as shown here:

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'bookr',
        'USER': <username>,
        'PASSWORD': <password>,
        'HOST': <host-IP-address>,
        'PORT': '5432',}}

```

Here, changes have been made to ENGINE to use PostgreSQL. The host IP address and port number of the server need to be provided for HOST and PORT, respectively. As the names suggest, USER is the database username, and PASSWORD is the database password. In addition to changes in the configuration, we will have to install the database drivers or bindings along with the database host and credentials. This will be covered in detail in later chapters, but for now, since we are using SQLite3, the default configuration will be sufficient. Note that the preceding code is just an example to show the changes you’ll need to make to use a different database such as PostgreSQL, but since we are using SQLite, we will use the database configuration that exists already, and there is no need to make any modifications to the database settings. Next, we will briefly read about Django apps and some of the default apps present in Django.

# Django apps

A Django project can have multiple apps that often act as discrete entities. That’s why, whenever required, an app can also be plugged into a different Django project. For example, if we develop an e-commerce web application, the web application can have multiple apps, such as a chatbot for customer support or a payment gateway to accept payments as users purchase goods from the application. These apps, if needed, can also be plugged into or reused in a different project.

Django comes with the following apps enabled by default. The following is a snippet from a project’s settings.py file:

```
INSTALLED_APPS = [
    'django.contrib.admin',

```