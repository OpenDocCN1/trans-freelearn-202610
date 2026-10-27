
The second type of assignment is the block assignment, where you just declare the variable name in the set statement, and the variable then gets assigned the result of the elaboration of the code inside the set block:

```
{% set date_filter %}
  DATE between {{ start_date }} and {{ end_date }}
{% endset %}
```

This second type of assignment is very handy to assign longer texts, especially if you want them properly formatted, as the block will keep the correct new lines.

We tend to use this kind of assignment when we want to incorporate variables inside a text, as inside a block, you can just type the text normally and output the variable or the result of a calculation, using the usual expression format with the two curly braces.

This is a simple and readable way to assign a variable with the text of a SQL query, or a filter to be passed in a macro.

## run_query() function

The run_query() Jinja function provided by dbt allows us to execute a piece of SQL toward a database that is configured for the environment in which we are executing the call.

You do not need to use this function until you start writing more complex functions, where the behavior depends on the content of the database or when you want to execute SQL commands outside of the normal dbt run cycle.

The following sample code from the dbt reference of the function gives us two useful inputs to discuss:

```
{% set results = run_query('select 42 as the_answer') %}
{% do run_query('DROP TABLE IF EXISTS PUBLIC.MY_USELESS_TABLE') %}
```

- The first line shows that you can use an assignment to execute a function and get the result in a variable, if you want to store the result in a variable.
- The second line shows that, as an alternative, you can use the expression statement that uses the `do` tag to execute an expression without printing out any result.

  This is useful to execute expressions or functions with side effects if you do not want to store the result; examples include changing a list or running some DDL or DML SQL code.

We will generally use the run_query function only when we want to perform some operation – that is, no SELECT queries – and we do not want to use the result of the query as the result is an Agate table – that is, a complex object that is not so easy to manipulate.

When you want to use the results of your query in a macro, we suggest using the get_query_results_as_dict() macro from the dbt_utils library, which we will introduce in the next section. That macro will return an easy-to-use Python dictionary.

We will use the `run_query` function in conjunction with the `run-operation` dbt command when we want to run DDL or DML commands in an automated way, such as when we want to define external tables, create landing tables, or ingest data directly from files using the `COPY INTO` command.

## The `return()` function

We will use the `return()` function only in more advanced macros that don’t directly produce the final output, instead returning a data structure for further use, typically a list or a dictionary, as shown in the following example macro:

```jinja
{%- macro get_user_dictionary() -%}
{% set users_dict_yaml -%}
developers:
  - MY_SF_USER
  - ANOTHER_SF_USER
readers:
  - PBI_USER
{%- endset %}
{% do return(fromyaml(users_dict_yaml)) %}
{%- endmacro %}
```

The preceding macro allows you to create and maintain a dictionary using the YAML format, by writing it inside the block that is assigned to the `users_dict_yaml` variable.

The variable is then passed to the dbt `fromyaml` function that parses the text and builds the corresponding data structure. A similar function, `fromjson`, exists to parse the JSON format.

Finally, the `return()` function returns the data structure to the caller.

Here, we saw an example that returns a manually maintained configuration; the other common use of the return function is to return data that you have loaded through the `run_query()` function (or the other more usable variations provided by libraries).

Until now, we have been writing our models and macros from scratch – that is, by writing all the code without any help besides the base dbt functionality. In the next section, we will see that we can get a lot of help from dbt libraries, called packages, that we can import into our projects.

## Building on the shoulders of giants – dbt packages

We have been writing our models and macros using only the base dbt functionality. In this section, we will see some existing libraries, called packages in dbt, and how we can import them to increase our productivity.

Using only dbt or Java or Python standard functionalities, you can build everything that you need. Doing everything by yourself does not pose an absolute limitation, but in all languages, the availability of libraries, collections of well-tested code to do frequent or complex tasks that you can easily download and use to accomplish complex tasks, has greatly augmented the productivity of programmers and their ability to solve more complex problems.

Using libraries, you can concentrate on the core of your problem and rely on a lot of other smart people’s knowledge and experience to reduce the complexity of your project, saving a lot of time.

You can reuse the code written by experts in areas that your team is not expert in, use proven solutions that you might otherwise be unable to create yourself, or just use quick and standard solutions to implement everyday needs with proven solutions instead of reinventing the wheel.

### Creating dbt packages

Maybe you are wondering how to create a dbt package.

You may be surprised to hear that you have already done it!

A **dbt package** is just a normal dbt project, like any other, and is usually stored in a git repository.

What makes a dbt project a package? Nothing really – just the fact of being imported by another package. The *package* name refers to the fact that the imported project packages some functionalities together that other projects can reuse.

A package can contain everything that a dbt project contains – models, macros, sources, tests, and so on – that will become part of the importing project, so you will be able to reference those elements as usual.

Therefore, you can reference an imported model with the `ref()` function and call a macro with its name, prefixed with the imported package name to avoid name clashes.

When you import a package, all dbt commands will work both on your code and the code that you have imported. As an example, when you run the `dbt run` command, the eventual models from the imported packages will also be run.

You can generally find or create packages to solve common problems such as the following:

- Transforming data from standard interfaces, such as SaaS and common products
- Libraries of macros to perform common tasks or tasks related to a specific domain
- Managing or auditing specific tools

### Importing dbt packages

- Using local folders, with the following syntax:

  ```yaml
  packages:
    - local: /your/local/folder  # a local path
  ```

  This is a special way to import packages and is very useful for packages that you are developing, as the changes will take effect as soon as you save the files of the package, without the need to run `dbt deps` at every change.

  This feature is available only in dbt Core and not in dbt Cloud, as you have no access to the local filesystem of your dbt Cloud projects.

- From Git private repositories: The exact syntax is slightly different, depending on the Git service provider that you are using, as you need to use the correct HTTP URL format to provide the access token and, eventually, the username, as required by your service provider:

  ```yaml
  packages:
    # github format
    - git: "https://<application token>@github.com/..."
    - git: "https://<user_name>:<personal token>@github.com/..."
    # Azure format
    - git: "https://<personal access token>@dev.azure.com/..."

    # BitBucket format
    - git: "https://<user>:<personal token>@bitbucketserver.com/..."
    # GitLab format
    - git: "https://<user>:<personal token>@gitlab.xxx.com/..."
  ```

  As you can see in the preceding examples, you provide the access information before an @ sign and the usual full repository URL.

  The formats to provide the access information are essentially twofold – one with just an access token and another with a username and access token, separated by a colon.

  To avoid putting secrets in clear into your repository, you should not write usernames and tokens directly in your YML file. Instead, you should use environment variables to store the actual values and use the `env_var` function in the YML file.

  Looking at the preceding examples, you can make the following substitutions:

  Replace `<user_name>` with `{{ env_var('DBT_ENV_SECRET_GIT_USERNAME') }}`.

  Replace `<token>` with `{{ env_var('DBT_ENV_SECRET_GIT_CREDENTIAL') }}`.

  Remember that in dbt Cloud, the environment variables have strict names that must start with `DBT_` or `DBT_ENV_SECRET_`. The difference between the two is the ability of dbt Cloud to remove its printed values from logs and not show or print out the secret ones in the UI.

Now that we have seen how to import a package, let’s discuss when to use each method.
