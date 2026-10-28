# Further reading

We only covered a small portion of the Zabbix API in this chapter; there's lots and lots more. If you plan to use it, consult the official Zabbix manual for information on all the methods, their parameters, and object properties. At the time of this writing, the Zabbix API manual can be found at `https://www.zabbix.com/documentation/3.0/manual/api`—but even if that changes, just visit `https://www.zabbix.com`, and look for the documentation.

# Summary

In this chapter, we dived deeper into the internal data structures Zabbix uses. While that's still just a small part of a large amount of database, XML import/export, API, and other information, it should help with some of the common problems users encounter at first.

We figured out how to get raw data from the frontend, which is the easiest method for small datasets. For bigger amounts of data, we learned to grab data from different history tables depending on data type. We also found out how Zabbix proxies keep data in their local databases. For situations where less precision is needed, we learned about the trends table and the calculation of the hourly minimal, maximal, and average values that are stored there. We also covered resetting user passwords directly in the database and fixing item history values if the item configuration was incorrect initially.

We explored the Zabbix XML import/export functionality, which allowed us to add and partially update hosts, templates, network maps, screens, host groups, images, and value maps. We looked at the XML format in brief and created a simple script to generate hosts from a CSV file.

And in the end, we looked at the Zabbix API, which allows us to control almost all of the Zabbix configuration. We logged in, controlled the host status, added and deleted a host, created a value map and retrieved some historical item values, and formatted the output a bit with the `json_pp` tool. Although the API was really great, we also discussed various issues with it, including the lack of auditing, proper validation, and error messages. While we could only cover a small part of the Zabbix API here, we figured out how to find out further information in the Zabbix manual and step up the API usage by using a Perl library. We also discovered the list of API libraries for various languages at `http://zabbix.org/wiki/Docs/api/libraries`.

We will continue diving into Zabbix in the next chapter. Various maintenance-related topics will be covered, including internal monitoring to find out cache usage and process busy rates, backing up our Zabbix configuration, and upgrading Zabbix when new versions come out. We will also explore all the parameters in the daemon configuration files.