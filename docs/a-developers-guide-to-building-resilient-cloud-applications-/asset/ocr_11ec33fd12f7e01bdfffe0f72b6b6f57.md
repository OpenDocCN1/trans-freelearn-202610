4. When the project is created, open program.cs to see the API implementation:

```
app.MapGet("/weatherforecast", (HttpContext httpContext) =>
{
    var forecast = Enumerable.Range(1, 5).Select(index =>
        new WeatherForecast
        {
            Date = DateTime.Now.AddDays(index),
            TemperatureC = Random.Shared.Next(-20, 55),
            Summary = summaries[Random.Shared.Next(summaries.Length)]
        })
        .ToArray();
    return forecast;
})
.WithName("GetWeatherForecast");
```

Figure 2.22 – Minimal API sample in Program.cs

After exploring the minimal web API, we will learn how we can import it into the API Management service in the next section.

# Importing an API to API Management

Let's look at the steps to import an API to API Management:

+   1. In the Azure portal, search for and select API Management services.
+   2. Next, on the API Management window, select the API Management instance you have already created.
+   3. Select APIs in the API Management services navigation pane.

![](bbox=[0.4, 0.57, 0.58, 0.76])

Figure 2.23 – APIs in API Management