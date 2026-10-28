
# Simplifying Complex Configurations with Templates

Make sure the **Group** dropdown says either **all** or **Linux servers** and click on **A test host** in the **NAME** column. We finally get to use the **Templates** tab—switch to it. Start typing c in the **Link new templates** input field. In the dropdown, our new template, **C_Template_Linux**, should be the very first one—click on it. Even though it might seem that this template is now added, it actually is not—if we would update the host now, it would not be linked:

![](img/f5313a804532fabacc8d07cc8607eeba_298_0.png)

Click on the **Add** control just below the template name. This form can be highly confusing, so try to remember that you have to do that extra click here. With the template added to the list, notice that it's actually a link. Clicking it will open template properties in a new window. When looking at host properties, this offers quick access to template properties. Such convenient links are available in many places in the Zabbix frontend:

![](img/f5313a804532fabacc8d07cc8607eeba_298_1.png)

In the end, click on the **Update** button at the bottom. Let's find out what this operation did—click on the **Details** link in the upper-left corner. In the expanded **Details** panel, we can see the operations that took place. In this case, some items were created and some were updated:

![](img/f5313a804532fabacc8d07cc8607eeba_298_2.png)

> When a template is linked to a host, identical entities that already exist on the host are linked against the template, but no historical data is lost. Entities that exist on the template only are added to the host and linked to the template.

Scrolling down a bit further, you'll be able to see that, same thing happened with triggers:

- Updated: Trigger "Web service is down" on "A test host".
- Updated: Trigger "SMTP service is down" on "A test host".
- Created: Trigger "SNMP trap has arrived on {HOST.NAME}" on "A test host".

Now this all seems like quite a lot of work for nothing, but if we had to add more hosts with the same items and triggers, without templates each host would require tedious manual copying, which is quite error-prone. With templates all we have to do is link the template to the freshly added host and all the entities are there automatically.

> Do not confuse templates for host groups. They are completely different things. Groups serve for a logical host grouping (and permission assigning), but templates define what is monitored on a host, what graphs it has, and so on. What's more, a single host group can contain both ordinary hosts and templates. Adding a template to a group will not affect hosts in that group in any way, only linking that template will. Think of groups as a way to organize the templates the same way as hosts are organized.

Now we could check out how linked items appear in the configuration. Open **Configuration | Hosts**, click on **Items** next to **A test host**:

| NAME | TRIGGERS | KEY |
|---|---|---|
| C_Template_Linux: CPU load | Triggers 1 | system.cpu.load |
| C_Template_Linux: Experimental SNMP trap |  | netSnmpExperimental2 |
| Full OS name |  | system.uname |
| C_Template_Linux: ICMP ping performance |  | icmppingsec |

We now want to make this particular item for this host slightly different from all other hosts that the template will be linked to, so let's change these things:

- **Update interval:** 360
- **History storage period:** 60

When you are done, click on **Update**. Now this host will have two parameters for a single item customized, while all other hosts that will get linked against the template will receive values from the template. Let's link one more host to our template now. Navigate to **Configuration | Templates**. Here we can see a full list of templates along with the hosts linked to them. The linkage area in this screen shows various entries and listed entities there have different color:

| TEMPLATES ▲ | APPLICATIONS | ITEMS | TRIGGERS | GRAPHS | SCREENS | DISCOVERY | WEB | LINKED TEMPLATES | LINKED TO |
|---|---|---|---|---|---|---|---|---|---|
| C_Template_Linux | Applications | Items 14 | Triggers 5 | Graphs | Screens | Discovery | Web |  | A test host |
| Template App LDAP Service | Applications 1 | Items 1 | Triggers 1 | Graphs | Screens | Discovery | Web |  |  |
| Template App MySQL | Applications 1 | Items 14 | Triggers 1 | Graphs 2 | Screens 1 | Discovery | Web |  | Zabbix server |

- **Gray:** Templates
- **Blue:** Enabled hosts
- **Red:** Disabled hosts

Click on **C_Template_Linux** in the **TEMPLATES** column. That provides us with a form where multiple hosts can be easily linked against the current template or unlinked from it. In this case we want to link a single host. In the **Hosts | Templates** section, choose **Linux servers** in the **Other | Group** dropdown, mark **Another host** in that box and click on the ◄ button:

![](img/f5313a804532fabacc8d07cc8607eeba_301_0.png)

## Macro usage

We previously added triggers from **Another host** to our template, but we didn't do that for **A test host**. Let's find out whether it has some triggers we could use in the template. Click on **Triggers** in the **Navigation** bar above the **Items** list. From the directly attached triggers in the list (the ones not prefixed with a template name), one is a trigger that takes into account items from two different hosts and we avoided copying it over before. The other directly attached triggers are the ones that we are interested in. Mark the checkboxes next to the **CPU load too high on A test host for last 3 minutes** and **Critical error from SNMP trap** triggers in the **NAME** column, then click on the **Copy** button at the bottom. In the next window, choose **Templates** in the **Target** type dropdown, **Custom templates** in the **Group** dropdown, then mark the checkbox next to the only remaining target (**C_Template_Linux**) and click on **Copy**. This time our copying had a bit more interesting effect, so let's expand the **Details** box again:

![](img/f5313a804532fabacc8d07cc8607eeba_304_0.png)

Two triggers we copied are added to the template. This causes the following:

- As **A test host** is linked to the modified template and it already has such triggers, these two triggers for that host are updated to reflect the linkage
- **Another host** does not have such triggers, so the triggers are created and linked to the template

While we are still in the trigger list, select **Another host** in the **Host** dropdown. Look carefully at the CPU load trigger that was added to this host in the previous operation:

![](img/f5313a804532fabacc8d07cc8607eeba_304_1.png)

Wait, that's definitely incorrect. The trigger refers to `A test host`, while this is `Another host`. The trigger name was correct when we first added it, but now the same trigger is applied to multiple hosts. In turn, the reference is incorrect for all the hosts except one. Let's try to fix this. Select **Custom templates** in the **Group** dropdown, then click on the **CPU load too high on A test host for last 3 minutes** trigger in the **NAME** column. Change the **Name** field to `CPU load too high on {HOST.NAME} for last 3 minutes`.

Yes, that's right, macros to the rescue again.

> The use of the word macros can be confusing here—Zabbix calls them macros, although they might be more correctly considered to be variables. In this book, we will follow Zabbix terminology, but feel free to read macro as variable.

Now click on **Update**. In the trigger list for the template, the trigger name has now changed to **CPU load too high on {HOST.NAME} for last 3 minutes**. That's not very descriptive, but you can expect to see such a situation in the Configuration section fairly often—Zabbix does not expand most macros in configuration. To verify that it is resolving as expected, navigate to **Monitoring | Triggers** and expand the filter. Set the **Triggers** status dropdown to **Any** and enter `CPU` in the **Filter by name** field, then click on the **Filter** button below the filter:

| HOST | NAME |
|---|---|
| A test host | CPU load too high on A test host for last 3 minutes |
| Another host | CPU load too high on Another host for last 3 minutes |

Notice how the trigger name includes the correct hostname now. In most cases, it is suggested to include a macro such as this in trigger names to easily identify the affected host.

The macro we used here, `{HOST.NAME}`, resolves to the host's visible name. We had no visible name specified and the hostname was used. If a host had the visible name defined, we could also choose to use the hostname with a macro `{HOST.HOST}`.

## User macros

The macros we used before are built-in. Zabbix also allows users to define macros and use them later. In this case it might be even more important to call them variables instead, so consider using that term in parallel. Let's start with a practical application of a user macro and discuss the details a bit later.
