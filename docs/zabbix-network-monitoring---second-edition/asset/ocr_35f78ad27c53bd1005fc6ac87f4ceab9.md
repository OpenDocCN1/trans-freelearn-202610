Before continuing, make sure that the SMTP server is running on Another host, and then click on Add. Let's find out what it looks like in the overview now: go to Monitoring | Overview, and make sure the Type dropdown has Triggers selected. Then, expand the filter, choose Any in the Triggers status dropdown, and click on Filter:

![](bbox=[0.14, 0.24, 0.82, 0.38])

Great, we can see that both hosts now have a trigger defined. Since the triggers differ, we also have two unused cells:

![](bbox=[0.17, 0.48, 0.82, 0.6])

Let's look at the trigger expression in more detail. It starts with an opening curly brace, and the first parameter is the hostname. Separated with a colon is the item key — net.tcp.service[smtp] here. The item key must be replicated exactly as in the item configuration, including any spaces, quotes, and capitalization. After the exact item key comes a dot as a separator, which is followed by the more interesting and trigger-specific thing: the trigger function. Used here is one of the most common functions, last(). It always returns a single value from the item history. There are trigger functions that require at least some parameter to be passed, but for the last() function, this is optional, and if the first parameter is just a number, it is ignored.

> Older versions of Zabbix required some parameter to be passed, even if it would have been ignored. It is still common to see syntax such as last(0) being used. Thus, last(300) is the same as last(0) and last() — they all return a single last value for one item.