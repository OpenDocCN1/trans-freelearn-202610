7. Enter the **FlightDescription** details as shown in the given screenshot and click the **Create** button. The newly created **FlightDescription** instance is displayed in the list of **FlightDescription** instances, as shown here:

| Origin | Destination | Price | Actions |
|---|---|---|---|
| NYC | DELHI | 1200 | View Delete |

8. The **View** button corresponding to a **FlightDescription** record shows the **FlightDescription** details along with the **Flight** details, which are associated with the **FlightDescription** instance. The **Delete** button deletes the **FlightDescription** instance along with the associated **Flight** instances.

9. Click the **Home** link to go back to the home page of the application and select the **Manage Flights** link to view the **Flight** instances, as shown here:

| Flight name | Action |
|---|---|
| No record found | |

10. Select the **Create Flight** link to view the form for creating **Flight** instances, as shown here:

| Field | Value |
|---|---|
| Flight name | FLT-100 |
| Flight Description | NYC - DELHI for only 1200 USD |

11. Enter **FLT-100** as the value of **Flight name** field and select **NYC – DELHI for only 1200 USD** as the value of **Flight Description** field. Click **Create** button to create **Flight** instance (with **FLT-100** as the flight name), which is a child of **FlightDescription** instance (origin: NYC, destination: DELHI, price: 1200). Similarly, create flights with names **FLT-200**, **FLT-300**, and **FLT-400**. Now, if you go to the **FlightDescription** listing page and click the **View** button, then you'll see the newly created **Flight** instances as a child of the **NYC-DELHI-1200 FlightDescription** instance, as shown here: