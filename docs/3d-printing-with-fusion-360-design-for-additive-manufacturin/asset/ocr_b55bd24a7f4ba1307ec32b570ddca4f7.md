![](bbox=[0.12, 0.08, 0.85, 0.35])

Figure 10.15 – Geometry tab for bar supports on SLA/DLP printers

By default, Fusion 360 creates bar supports utilizing a bouquet/tree structure as shown in the previous figure. We can modify the type of bar support Fusion 360 generates, and how close those bars should be from each other while touching the part, within the General tab of the BAR SUPPORT dialog, as shown in Figure 10.16. By changing the Anchor Density input from the default option of Medium to Dense, we can add more touch points between the bar supports and the part. We can also change the shape of the tree/bouquet structure. The following figure shows the effect of turning off the tree structure by selecting None from the Type drop-down menu.

Fusion 360 also allows you to group bars together by activating the Groups checkbox. Figure 10.16 shows 5 bars grouped together with a Group Taper (%) input of 50 percent.

This means that approximately five bars on their way from the part to the build plate bunch up together toward a central point by the specified amount. If you print your part and supports with this setting, you can handle groups of five bars at a time and break them off the part during post-processing much easier.

In the General tab, you can also turn on the Brace option for bar supports. Figure 10.16 shows bars with K type bracing. Fusion 360 offers three different brace types that resemble the letters K, X and N. Using the additional options below the Brace checkbox, you can control how many bars you wish to link together, what the link angle should be between the brace and the main column of your bar support, and whether you want the connection between the brace and the main column of the bar support to be straight or curved.