
```
"content" : {
    "*/*" : {
        "schema": {
            "$ref": "#/components/schemas/AntiHeroDto" ...
// other created paths UPDATE DELETE and CREATE Inputs
```

Here, we can see that the generated JSON object displays the available endpoints in our project. It is shown together with the operation ID, which is the default name of the method; it also specifies the parameters with the types required and the possible responses of the endpoint.

We have used the default URL where OpenAPI can be accessed. We can change the URL using the application's `springdoc.api-docs.path` property.properties file. For example, we have set it to `springdoc.api-docs.path=rest-docs`, which means we can now access the JSON document using http://localhost:8080/rest-docs/.

We can also access the YAML version of the documentation by accessing http://localhost:8080/v3/api-docs.yaml.

With that, we have successfully generated the documentation for our endpoints using springdoc-openapi. In the next section, we will learn how to configure, access, and use Swagger UI.

## Setting up Swagger UI

**Swagger UI** is a documentation tool that allows users to call the available APIs in your project directly from a browser. This is a more interactive tool that enables a more detailed and practical use of the APIs. Swagger UI is also open source, enabling more communities to support the tool.

### Installing and using Swagger UI

Swagger UI is already included under the `springdoc-openapi-ui` dependency. We have already included the OpenAPI extension code by adding the following code:

```
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-ui</artifactId>
    <version>1.6.4</version>
</dependency>
```

The OpenAPI dependency includes the Swagger UI extension; we can access the UI with the following URL: http://localhost:8080/swagger-ui.html. This will open the Swagger UI's **OpenAPI definition** page:

![](img/7c3ddd2e5893c9b5a0220148ed886548_137_0.png)

Figure 6.1 – The Swagger UI's OpenAPI definition page

Here, we can see that our Swagger UI has been accessed successfully. Our created endpoints in the Spring Boot project are also displayed, together with their HTTP methods. Let’s discuss the different parts of the Swagger UI documentation.

The first thing we can see in Swagger UI is that it contains text input, alongside the Explore button. This is where Swagger UI gets the list of endpoints it will render. In the preceding example, we can see that the default value is v3/api-docs. This means that the JSON documentation we have generated using the OpenAPI library is being used by Swagger to get the available endpoints.

We can change this and access the URL that contains the OpenAPI documentation in JSON or YAML format. The next component we will see is the list of available endpoints in our project. In the preceding example, the five endpoints we have developed are listed in Swagger UI. This is not a list as the Swagger UI tool is interactive, allowing us to try the available endpoints.

![](img/7c3ddd2e5893c9b5a0220148ed886548_138_0.png)

Figure 6.2 – POST request for anti-heroes

We can expand the `/api/v1/anti-heroes` endpoint to create a new Anti-Hero object in our database since this is using the POST HTTP method. The schema of the object we need to pass in our request body is specified. It defines both the names and the types of properties. In this case, the Anti-Hero entity has the following schema:

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "firstName": "string",
  "lastName": "string",
  "house": "string",
  "knownAs": "string"
}
```

```
"lastName": "string",
"house": "string",
"knownAs": "string"
}
```

The possible responses are also specified in the example endpoint. The possible response has a status of 200, which specifies success. It will also return the newly created entity in the database.

We want to test the endpoint and insert some example data into the database. To do this, we must click the Try it out button, which can be found at the top right, and click the Execute button to call the endpoint. Once the API call is successful, we will see the following output:

![](img/7c3ddd2e5893c9b5a0220148ed886548_139_0.png)

Figure 6.3 – Response from the POST request

Here, we can see that the API returned successfully as it returned a Code of 200 and the newly created entity in the database.

We can check whether our endpoint has successfully inserted the data into our table by accessing pgAdmin, as shown in the following screenshot:

![](img/7c3ddd2e5893c9b5a0220148ed886548_140_0.png)

Figure 6.4 – Verifying whether data has been inserted from the POST request

In the preceding example, we can see that our data has been inserted successfully into our table. Now, we can test the other available endpoints by getting, updating, or deleting the data we have created in the database.

We have successfully navigated through the Swagger UI tool and interacted with the available endpoints, but we can also modify Swagger UI based on our preferences and requirements by using properties, similar to what we can do with the OpenAPI documentation. We can also modify the URL to access Swagger UI; for example, we can place `springdoc.swagger-ui.path=/{custom-path}.html` in the `application.properties` file.

The other behavior we can also modify is the sorting behavior of our endpoints. We can change how the endpoints are arranged in the list in terms of alpha (arranged alphanumerically) or method (arranged by methods), or we can use a custom function to change the sorting method. For this, we can place `springdoc.swagger-ui.operationsSorter=(sort behavior)` in the `application.properties` file.

In this example, we will use `springdoc.swagger-ui.operationsSorter=method`. We will see the following output:

![](img/7c3ddd2e5893c9b5a0220148ed886548_141_0.png)

Figure 6.5 – Endpoints arranged by methods

As we can see, our endpoints are now arranged by the HTTP methods.

## Displaying pagination information in Swagger UI

Swagger UI can also be integrated with endpoints that use pagination. We can specify the page number, the size of the list on each page, and the sorting expression. For us to integrate the pagination parameters in Swagger UI, we need to add the springdoc-open-data-rest dependency.

To add the library, we must add the following code to our `pom.xml` file:

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-data-rest</artifactId>
    <version>1.6.4</version>
</dependency>
```

After successfully adding the library, let’s modify our `getAntiHeroes` method under `AntiHeroesController` so that we have a working pagination feature:

```java
... import org.springframework.data.domain.Pageable;

@GetMapping
public List<AntiHeroDto> getAntiHeroes(Pageable pageable) {
    int toSkip = pageable.getPageSize() *
                 pageable.getPageNumber();

    var antiHeroList = StreamSupport
        .stream(service.findAllAntiHeroes().spliterator(), false)
        .skip(toSkip).limit(pageable.getPageSize())
        .collect(Collectors.toList());

    return antiHeroList
        .stream()
        .map(this::convertToDto)
        .collect(Collectors.toList());
}
```

Now, let’s expand `api/v1/anti-heroes`, which uses the GET method:

Figure 6.6 – GET method for anti-heroes

Here, we can see that the Parameters section has a pageable property and that we can specify the page we want to retrieve, the size of each page, and the sorting expression. Now, we can execute it to retrieve the data from the database.

## Summary

With this, we have reached the end of this chapter. Let’s recap the valuable things you have learned.

First, you learned about the available features and properties of springdoc-openapi and how to configure and use the OpenAPI specification to generate the JSON and YAML documentation for our API calls. You also learned how to access Swagger UI, an interactive documentation tool for calling APIs directly in a browser. We simulated this to send test calls and modify certain behaviors, such as the domain URL and the sorting order. This newly acquired knowledge for generating documentation for APIs is very useful in real-world applications. This knowledge is used by developers to easily identify the available APIs that can be consumed, as well as their parameters and object responses.

In the next chapter, we will learn about the concept of Cross-Origin Resource Sharing (CORS), Spring Security, and JSON Web Tokens (JWTs).
