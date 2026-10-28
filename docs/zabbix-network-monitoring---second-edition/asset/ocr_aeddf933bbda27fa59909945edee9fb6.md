# Showing data on a big display

While visualization on an individual level is important, the real challenge emerges when there's a need to create views for a large display, usually placed for helpdesk or technical operators to quickly identify problems. This poses several challenges.

# Challenges

Displaying Zabbix on a large screen for many people requires taking into account the display location, the experience level of the people who will be expected to look at it, and other factors that can shape your decisions on how to configure this aspect of information displaying.

# Non-interactive display

In the majority of cases, data displayed on such a screen will be non-interactive—people are expected to view it, but not click around. Such a requirement is posed because drilldown usually happens on individual workstations, leaving the main display accessible for others. Additionally, somebody could easily leave the main display in an unusable state, so no direct access is usually provided. This means that data placed on the display must not rely on the ability to view problem details. It should be enough for the level of technical support to gather the required knowledge.

# Information overload

Having to place all information regarding the well-being of the infrastructure of an organization can result in a cluttered display, where too many details in a font that's way too small are crammed on the screen. This is the opposite of the previous challenge—you would have to decide which services are important and how to define each of them. This will require you to be working closely with the people responsible for those services so that correct dependency chains can be built. This is the method used most often to simplify and reduce displayed data while still keeping it useful.

> Both of these challenges can be solved with careful usage of screens and slide shows that display properly dependent statuses. Do not rely on slide shows too much—it can become annoying to wait for that slide to come by again because it was up for a few seconds only and there are now 10 more slides to cycle through.

![](bbox=[0.153, 0.768, 0.204, 0.812])