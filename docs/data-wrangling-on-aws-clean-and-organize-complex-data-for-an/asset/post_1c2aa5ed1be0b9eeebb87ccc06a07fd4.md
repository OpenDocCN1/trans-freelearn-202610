
## Standard and custom installation with Lambda functions

For most use cases, including a situation such as the IT person from Project A processing small volume data, we will want to deploy and execute data integration code in a serverless environment. AWS Lambda is a serverless, event-driven compute service that lets you run code for virtually any type of application or backend service without provisioning or managing servers. Lambda comes preloaded with a wide variety of important libraries, but the user has to package the libraries that are not available in the default environment along with the code when it is deployed to a Lambda function.

For smaller libraries, users can package dependencies along with the code and upload it to a Lambda function. We will see how to create a ZIP archive containing the dependent libraries for a Lambda function (https://docs.aws.amazon.com/Lambda/latest/dg/python-package.html#python-package-create-package-with-dependency). To do this, create a new Python virtual environment and install the new packages in the virtual environment. This will install the dependent packages under a pip install path on your machine. You can then move those packages to the deployment folder of your Lambda function and archive them as a ZIP file to be uploaded into the Lambda function.

Lambda functions can also have layers, which are saved in a ZIP file with the additional code/libraries. It can also contain a custom runtime, data, or configuration file. Using layers in a Lambda function will reduce the size of the deployment package, and the same Lambda layer can be shared across multiple Lambda functions, promoting reusable code components. Also, this helps to separate the responsibility of managing code segments among different teams. In addition, when you deploy code and dependencies in a single ZIP file, the Lambda function is not editable in the console, adding more difficulty in code maintenance. Currently, Lambda layers can be created with `.zip` file archives.

With all the preceding content, where would we use Lambda layers? Consider a situation where there is a central data warehouse, and different project teams access data from it using Lambda functions as microservices, and use it for different purposes. There are requirements to provide access only to certain user groups within the organization and also to perform data masking when accessing data. The database administrators are also worried about too many concurrent users accessing the database and leaving idle open connections that could strain the database. The individual project teams are also concerned about using and maintaining database drivers to connect to the warehouse, and they don’t have experience in managing database connection state, configurations, and so on. The ideal solution is to create a Lambda layer to access the warehouse that meets all preceding requirements, which will be maintained by the database administration team, and individual teams can utilize that Lambda layer to develop their code in Lambda functions. This promotes code reuse, separation of responsibilities, and quicker development cycles.

In the same context, users can use AWS SDK for pandas from Lambda layers. There are two options for using AWS SDK for pandas in your Lambda code.

### Standard installation – a Lambda managed layer

Create a Python Lambda function in the AWS Console with a Python 3.9 runtime. Scroll down the Lambda function overview screen until you reach the bottom of the page, where you can find the **Layers** section:

![](img/e28602b44ec813a47df57c96b204412c_103_0.png)

You can then choose to add a layer; choose the **AWSSDKPandas-Python39** layer, as shown in the following screenshot. The version number might change in the future as new releases are added by the AWS team. This is a layer that the AWS team has curated for your usage, removing and optimizing the components, thereby reducing the package size.

### Custom installation

- **II. Using the link, clone the entire git repository to your local machine/EC2 for further development:**

```bash
(awspandas) $ git --version
git version 2.37.3
(awspandas) $ git clone https://github.com/aws/aws-sdk-pandas
Cloning into 'aws-sdk-pandas'...
remote: Enumerating objects: 13000, done.
remote: Counting objects: 100% (565/565), done.
remote: Compressing objects: 100% (264/264), done.
remote: Total 13000 (delta 388), reused 431 (delta 299), pack-reused 12435
Receiving objects: 100% (13000/13000), 8.00 MiB | 7.91 MiB/s, done.
Resolving deltas: 100% (9773/9773), done.
(awspandas) $
```

Figure 3.5: The awswrangler git copy

- **2. Go to the code base and explore the code.** You can make changes to the existing code and add additional modules for your project needs:
  - **I. Navigate inside the installation path for awswrangler and explore the different folders.** You should see the code under the folder named `awswrangler` and there are individual folders for different services.
  - **II. For the purpose of this learning, let us modify the `__init__.py` file**, which will be executed when we import `awswrangler` inside our Python code. Explore the modules that are imported for various AWS services/awswrangler functionalities. You can see an import statement that gets variables from the `__metadata__.py` file. Append a new variable to the import statement with a new variable, `__message__`, as shown here, and save the file.

```python
from awswrangler.__metadata__ import __description__, __license__, __title__, __version__, __message__
# noqa
from awswrangler.__config import config # noqa
```

Figure 3.6: The awswrangler init.py file

  - **III. Open the `__metadata__.py` file and change the `__description__` fields**, as shown here. Also, add another variable, `__message__`, which was imported into the `__init__.py` module in the previous step, and save the file.

```python
__title__: str = "awswrangler"
__description__: str = "Pandas on AWS. Updated for custom installation."
__version__: str = "2.17.0"
__license__: str = "Apache License 2.0"
__message__: str = "This was changed as a part of Learning exercise."
```

Figure 3.7: The awswrangler metadata.py file

- **3. Install `awswrangler` from the custom code that was modified on your local machine or EC2 instance:**
  - **I. Navigate to the top-level folder where the `awswrangler` code is available.**
  - **II. Execute the `pip3 install .` command**, as shown here. This will install `awswrangler` on your machine with the local code base that has your custom changes.

```bash
(awspanandas)$ cd aws-sdk-pandas
(awspanandas)$ ls
CODE_OF_CONDUCT.md             awswrangler              test_infra
CONTRIBUTING.md                building                 tests
CONTRIBUTING_COMMON_ERRORS.md  docs                       tox.ini
LICENSE.txt                    fix.sh                   tutorials
NOTICE.txt                     poetry.lock              validate.sh
README.md                      pyproject.toml
THIRD_PARTY.txt                test.sh

(awspanandas)$ pip3 install .
Processing /Users/ssmun/Documents/Initiatives/DataWranglerBook/code/aws-sdk-pandas
Installing build dependencies ... done
Getting requirements to build wheel ... done
Preparing metadata (pyproject.toml) ... done
Collecting boto-core<2.0.0,>=1.27.11
  Downloading botocore-1.27.82-py3-none-any.whl (9.2 MB)
                                         9.2/9.2 MB 25.5 MB/s eta 0:00:00
```

Figure 3.8: awswrangler installation on your local machine

- **4. Verify and start using the `awswrangler` custom installation:**
  - **I. Open a Python shell and import `awswrangler`.**
  - **II. Print the `__message__` and `__description__` variables** to see whether we have installed the `awswrangler` package from our local machine/EC2 instance with the changes that were made in the earlier steps.

```python
(awspanandas)$ Python3
Python 3.9.12 (main, Apr  5 2022, 01:53:17)
[Clang 12.0.0 ]:: Anaconda, Inc. on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import awswrangler as wr
>>> print(wr.__description__)
Pandas on AWS. Updated for custom installation.
>>> print(wr.__message__)
This was changed as a part of Learning exercise.
>>>
```

Figure 3.9: Verifying the awswrangler local installation with custom changes

- **5. As a final step, you can validate that the changes we made are only restricted to our virtual environment** by moving out of the virtual environment and installing the `awswrangler` package using the `pip3 install awswrangler` command (which will install the package from the PyPI remote repository without our custom code changes):

```bash
(aws pandas) $deactivate
$pip3 install awswrangler
Requirement already satisfied: awswrangler in /Users/ssmsun/miniconda3/lib/python 3.9/site-packages (2.16.1)

$python3
Python 3.9.12 (main, Apr 6 2022, 01:53:17)
[Clang 12.0.0 1:: Anconda, Inc. on darwin]
Type "help", "copyright", "credits" or "license" for more information.
>>> import awswrangler as wr
>>> print(wr.__description__)
Pandas on AWS.
>>> print(wr.__message__)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: module 'awswrangler' has no attribute '__message__'
>>>
```

![](img/e28602b44ec813a47df57c96b204412c_102_0.png)

We saw how to install the awswrangler package on a local machine/EC2 instance by simply using the pip command. We also downloaded the code from the awswrangler git repository, made the changes that are needed for our project, and installed the changed version of awswrangler. We also learned how to install packages inside virtual environments so that the changes made for one project do not conflict with other projects.

Congratulations on completing this step! You can play around with the package for some time here to understand more. Once you are done exploring, let us move to the next section, where we have to help an IT person install awswrangler with a Lambda function.
