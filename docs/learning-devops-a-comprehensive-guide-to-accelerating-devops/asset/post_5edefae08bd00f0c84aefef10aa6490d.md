

The following code illustrates a response time of less than 400 ms:

```
pm.test("Response time is less than 400ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(400);
});
```

The following code illustrates that the response in JSON format is not empty:

```
pm.test("Json response is not empty", function () {
    pm.expect(pm.response).to.be.json;
});
```

The following code illustrates that, in the JSON response, the `userId` property is equal to 1:

```
pm.test("Json response userId eq 1", function () {
    var jsonRes = pm.response.json();
    pm.expect(jsonRes.userId).to.eq(1);
});
```

And so, finally, the **Tests** tab of our request, which tests our API, contains all this code, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_376_0.png)

Figure 11.14 – Postman Tests code

We have completed our Postman request with test writing, which will check the proper functioning of the API according to its feedback code, performance, and response content.

> Note
> For more information about the Postman tests and scripts, you can read the documentation at https://learning.getpostman.com/docs/postman/scripts/intro_to_scripts.

In this section, we have just seen how, in Postman, we can write API tests to check the proper functioning of our API. We will now run our Postman request locally in order to test our API.

## Executing Postman request tests locally

So far, in Postman, we have created a collection in which two requests contain the parameters and tests of our APIs that are to be tested. To test the proper functioning of the APIs with their parameters and tests, we must now execute our requests that are in Postman. Note that it will only be at the end of this execution that we will know whether our APIs correspond to our expectations.

To execute a Postman request, perform the following actions:

1. You must first choose the desired environment.
2. Click on the **Send** button of the request, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_377_0.png)

Figure 11.15 – Postman signup

3. In the **Body** tab, we can then view the content of the query response, and if we want to display it in JSON format, we can choose the display format. The following screenshot shows the response of the request displayed in JSON format:

![](img/d3344b39c888aab0b832625465f837c9_378_0.png)

4. The **Test Results** tab displays the results of the execution of the tests that we previously wrote, and in our case, the four tests have been executed correctly—they are all green, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_378_1.png)

In the preceding screenshot, we can see that the return code of the Postman request is equal to 200, which corresponds to the request’s successful execution return code, and its execution time of 23 ms, which is below the threshold (400 ms) that I set for myself as an example.

In the event that one of the tests fails, it will be displayed in red to clearly identify it. An example of a failed test is shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_379_0.png)

Figure 11.18 – Postman test failed

We have just seen the execution of a Postman request to test an API, but this execution is only for the current request. If we want to execute all Postman requests in a collection, we can use **Postman Collection Runner**.

Postman Collection Runner is a Postman feature that automatically executes all the requests in a collection in the order in which they have been organized.

You can learn more about the Collection documentation by visiting `https://learning.getpostman.com/docs/postman/collection_runs/starting_a_collection_run/`.

The following two screenshots show the **Runner** execution steps, in which we choose the collection to execute, the environment, and the number of iterations. To start its execution, we click on the **Run DemoBook** button:

![](img/d3344b39c888aab0b832625465f837c9_380_0.png)

Figure 11.19 – Postman Runner

And so, in the **Runner** tab, we can see the test execution result of all the requests in the collection, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_380_1.png)

Figure 11.20 – Postman Runner execution

![](img/d3344b39c888aab0b832625465f837c9_381_0.png)

In this section, we have learned how to execute Postman requests in order to test an API in a unitary way, before executing all the requests in the collection using Postman Runner. In the following section, we will introduce Newman, which allows us to automate the execution of Postman tests.

## Understanding the Newman concept

So far in this chapter, we have talked about using Postman locally to test the APIs that we develop or consume. But what is important in the unit, acceptance, and integration tests is that they are automated so that they can be executed within a CI/CD pipeline.

Postman, as such, is a graphical tool that does not automate itself, but there is another tool called **Newman** that automates tests that are written in Postman.

![](img/d3344b39c888aab0b832625465f837c9_381_1.png)

Newman is a free command-line tool that has the great advantage of automating tests that are already written in Postman. It allows us to integrate API test execution in CI/CD scripts or processes.

In addition, it offers the possibility of generating the test results of reports of different formats (HTML, JUnit, and JSON).

Nevertheless, Newman does not allow us to do the following:

- To create or configure Postman requests; as we will see, requests that are executed by Newman will be exported from Postman.
- To execute only one request that is in a collection—it executes all the requests in a collection.

## Testing APIs with Postman

> Note
> To learn more about Newman, you can visit the product page at `https://www.npmjs.com/package/newman`.

To use Newman, we will need—as stated in the _Technical requirements_ section of this chapter—to have Node.js and npm installed, which are available at `https://nodejs.org/en/` (this installer installs both tools).

Then, to install Newman, we must execute this command in the terminal:

```
npm install -g newman
```

The following screenshot shows the execution of the command:

![](img/d3344b39c888aab0b832625465f837c9_382_0.png)

Figure 11.21 – Newman installation

This command installs the npm `newman` package and all its dependencies globally, that is, it is accessible on the entire local machine.

Once installed, we can test its installation by running the `newman --help` command, which displays the arguments and options to use, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_382_1.png)

Figure 11.22 – Newman help command

In this section, we introduced Newman by talking about its advantages, and we learned how to install it. In the next section, we will export Postman's collection and environment for use with Newman.

## Preparing Postman collections for Newman

As we have just seen, Newman is Postman’s client tool, and in order to work, it needs the configuration of the collections, requests, and environments that we have created in Postman.

That’s why, before running Newman, we will have to export Postman’s collection and environments, and this export will serve as Newman’s arguments. So, let’s start exporting the DemoBook collection that we created in Postman.

### Exporting the collection

The exporting of a Postman collection consists of obtaining a JSON file that contains all the settings of this collection and the requests that are inside it.

It is from this JSON file that Newman will be able to run the same API tests as when we ran them from Postman.

To do this export, perform the following tasks:

1. Go to the context menu of the collection that we want to export.
2. Choose the **Export** action.
3. Then, in the window that opens, uncheck the **Collection v2.1 (recommended)** checkbox.
4. Finally, validate by clicking on the **Export** button.

