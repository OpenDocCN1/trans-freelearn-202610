Figure 3.27: awswrangler glue installation – verifying the awswrangler version

In order to use newer versions of awswrangler, follow the steps similar to the Python 3.6 shell to add a new wheel file. The following screenshot shows the output of uploading awswrangler version 2.17 to the Glue Python shell, which is a more recent version compared to awswrangler 2.15.1

```
pyparsing-3.0.9 python-dateutil-2.8.2 python-utils-3.3.3 pytz-2022.2.1 redshift-connector-2.0.909 requests-2.28 s3transfer-0.6.0 scramp-1.4.1 setuptools-65.4.0 six-1.16.0 soupsieve-2.3.2.post1 urllib3-1.26.12 yarl-1.8.1
```

```
[notice] A new release of pip available: 22.1.2 -> 22.2.2
[notice] To update, run: pip install --upgrade pip
```

```
Setup complete. Starting script execution:
----
2.17.0
```

Figure 3.28: awswrangler glue installation – verifying the awswrangler version (a custom version installation)

# Custom installation of awswrangler for Python shell jobs

We will utilize the poetry library to create a custom wheel file from the awswrangler repository where we made our changes. We will utilize the awswrangler local repository that we utilized for our local machine/EC2 instance setup:

+   1. You can install Poetry using the pip3 install poetry command in your Python virtual environment.
+   2. Execute the build_wheel.sh script from the local repository to build the wheel file, as shown in the following figure, and the wheel file will be created using the poetry.lock configurations, provided along with the awswrangler repository.