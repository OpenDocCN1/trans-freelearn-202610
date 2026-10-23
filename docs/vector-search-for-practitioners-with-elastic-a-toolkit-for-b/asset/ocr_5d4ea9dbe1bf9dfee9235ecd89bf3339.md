Note that instead of applying the first approach and trying to generate vectors directly from the logs, we will adopt the strategy of expanding them to a human-readable description first, allowing us to avoid the intensive process of model training.

We are now going to learn how to generate synthetic logs.

# Synthetic log

With synthetic logs, we enable users to test the entire pipeline without having to generate logs. Synthetic logs can be considered the same as the mocks used in any development project.

To be able to run the process for the purpose of testing in this book, we have prepared a block of code that will help generate synthetic logs. The following code blocks respectively generate Apache HTTP access logs, nginx, Syslog, AWS CloudTrail, Microsoft event logs, Linux audit logs, and more by heavily leveraging the Faker library.

# *Apache HTTP server (common log format)*

The following code generates Apache HTTP Server log, we will see how to use the same code structure for all other log formats subsequently:

```
fake = Faker() #we use a single instance for the rest of the code
def generate_apache_log():
    return
        '{RemoteHost} - - [{Timestamp}]
        "{RequestMethod}{RequestURI} {Protocol}" {StatusCode}
        {ResponseSize}'.format(RemoteHost=fake.ipv4(),
            Timestamp=fake.date_time_this_year()
                .strftime('%d/%b/%Y:%H:%M:%S %z'),
            RequestMethod=fake.http_method(),
            RequestURI=fake.uri(),
            Protocol='HTTP/1.1',
            StatusCode=random.choice([200, 404, 500]),
            ResponseSize=random.randint(100, 10000))
```

# *nginx (combined log format)*

```
def generate_nginx_log():
return '{RemoteAddress} - {RemoteUser} [{Timestamp}] "{RequestMethod} {RequestURI} {Protocol}" {StatusCode} {ResponseSize} "{Referer}" "{UserAgent}"'.format(
RemoteAddress=fake.ipv4(),
RemoteUser='-',
Timestamp=fake.date_time_this_year().strftime('%d/%b/%Y:%H:%M:%S %z'),
RequestMethod=fake.http_method(),
```