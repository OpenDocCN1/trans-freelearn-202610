+   6. Click on **Color**, change the opacity to **50%**, and choose a gray color

The **Navy Pier** coordinates are shown in the URL in the following figure (**41.8859088,-87.6064094**):

![](bbox=[0.136, 0.121, 0.913, 0.151])

![](bbox=[0.136, 0.169, 0.913, 0.63])

Figure 11.57: Navy Pier

+   7. Create a calculated field as follows:

```
MAKEPOINT(41.892133, -87.604045)
```

+   8. Drag the **Navy Pier** field onto the map on top of the **Add a Marks Layer** pop-up

+   9. Change the **Marks** type to **Map**

+   10. In order to connect **Navy Pier** to our **Address manual** point and measure the distance, create another calculated field called Line:

```
MAKELINE([Address manual], [Navy Pier])
```