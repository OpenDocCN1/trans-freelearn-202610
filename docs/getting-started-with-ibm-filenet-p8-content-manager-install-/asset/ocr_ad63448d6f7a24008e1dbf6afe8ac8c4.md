# Server-side sorting

It is a CE requirement to configure TDS to allow server-side sorting of search results. This is usually enabled by default, but you can check it via the TDS web administration application. Navigate to **Server administration | Manage server properties | Search settings** and make sure that the checkbox for **Allow only administrators to perform sort searches** is *not* checked. You don’t want to restrict it to administrators; you want to allow anyone to do sorted searches.

# The suffix

TDS can hold arbitrarily many logical partitions of data, each falling within a unique **suffix**. A suffix is just a collection of attribute values under which all other entries fall. Our organization refers to the enterprise directory as "WhoZit?", so we will make our suffix be dc=whozit,dc=example,dc=net.

> Directory concepts and notation can seem pretty odd. Most people don’t encounter them every day. There is a lot of material available on the web to explain both the concepts and the notation. Here is one example that is clearly written and oriented toward directory novices: http://www.skills-1st.co.uk/papers/ldap-schema-design-feb-2005/index.html.

You can add a suffix to TDS via the web-based administration tool. Navigate to **Server administration | Manage server properties | Suffixes**. Type the full suffix into the **Suffix DN** box and click the **Add** button. Be sure to also click the **OK** or **Apply** button to commit the change (it’s easy to miss).

> You might also have to restart the directory server before the new suffix will show up in the directory. Navigate to **Server administration | Stop/start/restart server.**