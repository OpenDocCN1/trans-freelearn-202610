
## Filtering discovery results

Looking at the items, triggers, and graphs that were created, besides real interfaces, the loopback interface also got discovered, and all of those entities got created for it. In some cases, it would be useful to monitor that interface as well, but for most systems, such data would not be useful.

If we look at the list of items in the configuration, the LLD-generated items have the checkbox next to them disabled, and we can't click on them to edit properties directly either. The controls in the **STATUS** column allow us to enable or disable them individually, though. LLD-generated items on a host cannot be edited, except for being disabled or enabled. Note that in the frontend, this can only be done one by one for each item—we cannot use mass update as the checkboxes are disabled.

Disabling an LLD-generated item on many hosts could be a massive manual task. We could think about disabling the prototype, but that would not work for two reasons. Firstly, we only want to disable items for the loopback interface, but the same prototype is used for items on all interfaces. Secondly, state changes in the prototype are not propagated to the generated items. The initial state in which these items are created—enabled or disabled—will be kept for them.

What about other changes to these items, such as changing the item key or some other property? Those would get propagated downstream, but only when the discovery itself was run by the Zabbix server, not when we made the changes to the prototype in the frontend. In practice, this means that we would have to wait for up to the LLD rule interval to see these changes applied downstream.

Luckily, there's a way to easily avoid creating items for some of the discovered entities, such as in our case: not creating items for the loopback interface. This is possible by filtering the entities' LLD returns on the LLD rule level. Let's change our existing rule to ignore interfaces with the name lo.

> If we wanted to keep LLD-generated items but disable or enable several of them, in some cases, that might be worth doing via the Zabbix API—we will have a brief introduction to the API in *Chapter 21, Working Closely with Data.*

![](img/f5313a804532fabacc8d07cc8607eeba_467_0.png)

## Automating Configuration

Navigate to **Configuration | Templates**, and click on **Discovery** next to **C_Template_Linux**. Then, click on **Interface discovery** in the NAME column. Notice how there's another tab here: **Filters**. Switch to that tab, and in the first and only **Filters** entry, fill in the following:

- **MACRO:** `{#IFNAME}`
- **REGULAR EXPRESSION:** `^([^1].*|[^lo].+)$`

![](img/f5313a804532fabacc8d07cc8607eeba_468_0.png)

When done, click on **Update**. LLD filters work by only returning matching entries. In this case, we wanted to exclude the entry `lo` and keep everything else. Unfortunately, Zabbix daemons only support POSIX extended regular expressions—in this flavor, negating a string is fairly complicated. The filter we used will exclude `lo` but match everything else—including `eth0`, `enp0s8`, and `loop0`.

> 💡 We will explore a way to negate strings in an easier way later in this chapter.

To see whether this worked, navigate to **Configuration | Hosts** and click on **Items** next to **A test host**. In the list, notice how both `lo` interface items have an orange icon with an exclamation mark in the **INFO** column. If you move the mouse cursor over it, a message explains that this item is not discovered anymore and will be deleted at some later time:

![](img/f5313a804532fabacc8d07cc8607eeba_468_1.png)

In this case, the item is not discovered because it got excluded by the filter, but the reason does not matter that much—it could be an interface being removed or having its name changed as well. But why will it be removed after that specific amount of time, a bit more than 29 days? If we look at the properties of our LLD rule again, there's a field called **Keep lost resources period:**

![](img/f5313a804532fabacc8d07cc8607eeba_469_0.png)

Here, we may specify how long items will be kept for when they are not discovered again, and the default is 30 days. The tooltip helpfully told us how much time we have left before the item will be deleted and at what time exactly it will be deleted. Other entities, including triggers and custom graphs, are kept as long as the underlying items are kept.

> An LLD rule is only evaluated when it gets new data. If the rule stops getting data, items would tell you that they are supposed to be deleted, but they won't be deleted until the rule gets new data and is evaluated.

Now, navigate to **Monitoring | Latest data**, and click on **Graph** for **Incoming traffic on lo**. Let some time pass, and notice that items that are scheduled for deletion still continue collecting data. This might be undesirable, when we had initially been monitoring a lot of things on a device, overloaded it, and then applied filtering, hoping to remedy the situation. There is no way to directly control this, but we may temporarily set the resource-keeping to 0, which would remove the items that are not discovered anymore next time the LLD rule runs. In the LLD rule properties, set the value of this field to 0 and click on **Update**. After a couple of minutes, check the item list for **A test host** in the configuration—both of the automatic lo interface items should be gone now.

What if we would like to have a different set of items for different discovered entities, for example, monitoring more things on interfaces with a specific name? That is not easily possible, unfortunately. One way would be by creating two different LLD rules with different item prototypes, then filtering for one set of entities in one LLD rule, and another set in the other LLD rule. Still, that is more complicated than one might expect. LLD rules have the same uniqueness criteria as items: the key. With some items, we can use a little trick and have an item with a key called key and another with key[ ]. Specifying empty square brackets will denote empty parameters, but functionally, the item will be exactly the same. Unfortunately, the agent LLD keys do not accept parameters, so this trick won't work. One workaround would be specifying an alias on an item key—we will discuss how that can be done in *Chapter 22, Zabbix Maintenance*.

## Filesystem discovery

We have found out that a Zabbix agent has built-in support for discovering network interfaces. It can also discover other things, one of the most popular being filesystems. Before we configure that, let's find out what we can expect from such a feature.

## Introducing the LLD JSON format

The discovery does not just look a bit like an item in the frontend; it also operates in the same way underneath. The magic happens based on the contents of a specific item value. All the found things are encoded in a JSON structure. The easiest way to see what's returned is to use `zabbix_get` and query a Zabbix agent. On a test host, run this command:

```
```bash
$ zabbix_get -s 127.0.0.1 -k net.if.discovery
```
```

Here, `net.if.discovery` is just an item key, not different from other item keys. This will return a small string, similar to the following:

```
```json
{data:[{{#IFNAME}:enp0s3},{{#IFNAME}:enp0s8},{{#IFNAME}:lo}]}
```
```

While it's mostly understandable, it would be even better with some formatting. The easiest way is to use Perl or Python tools. The Python method would be this:

```
```bash
$ zabbix_get -s 127.0.0.1 -k net.if.discovery | python -mjson.tool
```
```

The Perl method would be one of these:

```
```bash
$ zabbix_get -s 127.0.0.1 -k net.if.discovery | json_pp
$ zabbix_get -s 127.0.0.1 -k net.if.discovery | json_xs
```
```

The latter method should be faster but requires the JSON::XS Perl module. For our purposes, performance should not be a concern, so choose whichever method works for you. The output will be similar to this:

```
```json
{
    data : [
        {
            {#IFNAME} : enp0s3
        },
        {
            {#IFNAME} : enp0s8
        },
        {
            {#IFNAME} : lo
        }
    ]
}
```
```
