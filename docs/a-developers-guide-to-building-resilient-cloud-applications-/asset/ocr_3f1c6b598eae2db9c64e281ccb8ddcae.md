```
var client = new EventGridPublisherClient(endpoint,
credential);
```

We add the following source code to create a new event1 variable and populate that variable with sample data:

```
var event1= new EventGridEvent(
    subject: $"New Patient: Hamida Rebai",
    eventType: "Patients.Registration.New",
    dataVersion: "1.0",
    data: new
    {
        FullName = "Hamida Rebai",
        Address = "Quebec, G2C0L6"
    }
);
```

Next, we add an event2 variable:

```
var event2 = new EventGridEvent(
    subject: $"New Patient: Mohamed Trabelsi",
    eventType: "Patients.Registration.New",
    dataVersion: "1.0",
    data: new
    {
        FullName = "Mohamed Trabelsi",
        Address = "Quebec, G1X3Z2"
    }
);
```

After, we will add the following lines of code to invoke the EventGridPublisherClient.SendEventAsync method, and we will see in the following source code how to render the message for every event to the console:

```
await client.SendEventAsync(event1);
Console.WriteLine("This is the first event published");
await client.SendEventAsync(event2);
Console.WriteLine("This is the second event published");
```