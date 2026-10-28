
After some 30 seconds, check the map in the monitoring view – status icons are not displayed in the configuration section:

![](img/f5313a804532fabacc8d07cc8607eeba_364_0.png)

As soon as a problem appeared on a host, the icon was automatically changed. In the configuration, there were two additional states that could have their own icons – when a host is disabled and when it is in maintenance. Of course, a server should not turn into a router or some other unexpected device. The usual approach is to have a normal icon and then an icon that has a red cross over it, or maybe a small colored circle next to it to denote the status.

> Notice how the link is not horizontally aligned anymore. As the icons are positioned by their top-left corner, a smaller icon had its center moved. The link is attached to the center of the icon.

Manually specifying different icons is fine, but doing that on a larger scale could be cumbersome. Another feature to identify problematic elements is called icon highlighting. As opposed to selecting icons per state, here a generic highlighting is used. This is a map-level option, there is no way to customize it per map element. Let's test it. In the list of all the maps, click on Properties next to First map and mark the checkbox **Icon highlighting**. This setting determines whether map elements receive additional visualization depending on their status. Click on Update, then open Configuration | Hosts. Click on Enabled next to Another host to toggle its status and acknowledge the popup to disable this host. Check the map in the monitoring view:

![](img/f5313a804532fabacc8d07cc8607eeba_364_1.png)

Both hosts now have some sort of background. What does this mean?

- The round background denotes the trigger status. If any trigger is not in the OK state, the trigger with the highest priority determines the color of the circle.
- The square background denotes the host status. Disabled hosts receive gray highlighting. Hosts that are in maintenance receive an orange background.

Click on **A test host**, then click on **Triggers**. In the trigger list, click on **No** in the **ACK** column, enter some message and click on **Acknowledge**. Check the map in the monitoring view again:

![](img/f5313a804532fabacc8d07cc8607eeba_365_0.png)

The colored circle now has a thick, green border. This border denotes the acknowledgment status—if it's there, the problem is acknowledged.

Zabbix default icons currently are not well centered. This is most obvious when icon highlighting is used—notice how **Another host** is misaligned because of that shadow. For this icon, it's even more obvious in the problem highlighting. In the constructor of the **First map**, click on **A test host** and choose **Default** in the **Problem** dropdown in the **Icons** section. Click on **Apply** and then on **Update** for the map, then check the map in the monitoring section view:

![](img/f5313a804532fabacc8d07cc8607eeba_365_1.png)

In such a configuration, another icon set might have to be used for a more eye-pleasing look.

> The Zabbix source archive has older icons, used before Zabbix 2.0, in the misc/images/png_classic directory.

To return things to normal state, open Configuration | Hosts, click on Disabled next to Another host and confirm the popup, then execute on "A test host":

```
$ touch /tmp/severity1
```

## Available map elements

Hosts are not the only element type we could add to the map. In the constructor for the First map, click on Another host and expand the Type dropdown in the element properties. We won't use additional types right now, but let's look at what's available:

![](img/f5313a804532fabacc8d07cc8607eeba_366_0.png)

- **Host**: We already covered what a host is. A host displays information on all associated triggers.

- **Map**: You can actually insert a link to another map. It will have an icon like all elements, and clicking it would offer a menu to open that map. This allows us to create interesting drilldown configurations. We could have a world map, then linked in continental maps, followed by country-level maps, city-level maps, data center-level maps, rack-level maps, system-level maps, and at the other end we could actually expand to have a map with different planets and galaxies! Well, we got carried away. Of course, each level would have an appropriate map or schematic set as a background image.

- **Trigger**: This works very similar to a host, except only information about a single trigger is included. This way you can place a single trigger on the map that is not affected by other triggers on the host. In our nested maps scenario we could use triggers in the last display, placing a core router schematic in the background and adding individual triggers for specific ports.

- **Host group**: This works like a host, except information about all hosts in a group is gathered. In the simple mode, a single icon is displayed to represent all hosts in the selected group. This can be handy for a higher-level overview, but it's especially nice in the preceding nested scenario in which we could group all hosts by continent, country, and city, thus placing some icons on an upper-level map. For example, we could have per country host group elements placed on the global map, if we have enough room, that is. A host group element on a map can also display all hosts individually — we will cover that functionality a bit later.

- **Image**: This allows us to place an image on the map. The image could be something visual only, such as the location of a conditioner in a server room, but it could also have an URL assigned; thus, you can link to arbitrary objects.

Talking about **URLs**, take a look at the bottom of the element properties popup:

![](img/f5313a804532fabacc8d07cc8607eeba_367_0.png)

Here, multiple URLs can be added and each can have a name. When a map is viewed in the monitoring section, clicking on an element will include the **URL** names in the menu. They could provide quick access to a web management page for a switch or a UPS device, or a page in an internal wiki, describing troubleshooting steps with this specific device. Additionally, the following macros are supported in the **URL** field:

- `{TRIGGER.ID}`
- `{HOST.ID}`
- `{HOSTGROUP.ID}`
- `{MAP.ID}`

This allows us to add links that lead to a Zabbix frontend section, while specifying the ID of the entity we clicked on in the opened URL.

## Map filtering

Map elements "host", "host group", and "map" aggregate the information about all the relevant problems. Often that will be exactly what is needed, but Zabbix maps also allow filtering the displayed problems. The available conditions are as follows:

- **Acknowledgment status**: This can be set for the whole map
- **Trigger severity**: This can be set for the whole map
- **Application**: This can be set for individual hosts

In the map properties, the **Problem display** dropdown controls what and how problems are displayed based on their acknowledgment status. This is a configuration-time only option and cannot be changed in the monitoring section. The available choices are as follows:

- **All**
- **Separated**
- **Unacknowledged only**

The All option is what we have selected currently and the acknowledgment status will not affect the problem displaying there. The **Separated** option would show two lines—one, displaying the total amount of problems and another displaying the amount of unacknowledged problems:

![](img/f5313a804532fabacc8d07cc8607eeba_368_0.png)

Notice the total and unacknowledged lines having different colors. The option **Unacknowledged only**, as one might expect, shows only the problems that are not acknowledged at this time.

Another way to filter the information that is displayed on a map is by trigger severity. In the map properties, the **Minimum trigger severity** option allows us to choose the severity to filter on:

![](img/f5313a804532fabacc8d07cc8607eeba_368_1.png)

If we choose **High**, like in the preceding screenshot, opening the map in the monitoring section would ignore anything but the two highest levels of severity. By default, **Not classified** is selected and that shows all problems. Even better, when we are looking at a map in the monitoring section, in the upper right corner we may change the severity, no matter which level is selected in the map configuration:

![](img/f5313a804532fabacc8d07cc8607eeba_368_2.png)
