These templates might still be missing a few interesting items, such as the expected NVPS item we created earlier. It is suggested to create a separate template with such missing things instead of modifying the default template. Such an approach will allow easier upgrades, as new versions could add more processes, caches, and have other improvements to the default templates. If we leave the default templates intact, we can import a new XML file, tell Zabbix to add all missing things, update existing things, and remove whatever is not in the XML, and we will have an up-to-date default template. If we had it modified...it could be a lot of manual work to update it.

# Unsupported items and more problems

We now know quite a bit about the internal monitoring of Zabbix, but there are still more possibilities. Unsupported items are no good, so let's discuss the ways we could monitor the situation with them.

# Counting unsupported items

Similar to cache usage and process busy rates, we may also monitor the count of unsupported items with an internal item. To create such an item, let's go to Configuration | Hosts, click on Items next to A test host, and then click on Create item. Fill in these values:

+   - **Name:** Amount of unsupported items
+   - **Type:** Zabbix internal
+   - **Key:** zabbix[items_unsupported]

When done, click on the Add button at the bottom. After a short while, check this item on the Latest data page:

Amount of unsupported items 2015-12-11 17:02:52 58

58? That is an extremely high value for such a small installation, although in this case it is caused by the VMware monitoring being down. At this time, a VMware timeout results in all VMware items becoming unsupported. In a perfect environment, there would be no unsupported items, so we could create a trigger to alert us whenever this item receives a value larger than 0. That wouldn't be too useful anywhere but in really small environments, though—usually, a thing becomes broken here or there, and the unsupported item count is never 0. A more useful trigger would thus be one that alerts about a larger increase in the number of unsupported items. The change() trigger function could help here:

```
{A test host:zabbix[items_unsupported].change()} >5
```