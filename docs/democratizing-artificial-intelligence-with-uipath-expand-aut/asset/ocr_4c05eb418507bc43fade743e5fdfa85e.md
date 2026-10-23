You can see the flow decision in the following screenshot:

![](bbox=[0.381, 0.123, 0.615, 0.285])

Figure 9.21 – Have input? flow decision

By setting **NewTransaction** equal to True, the **Have input?** flow decision within the **Main** workflow will return `True`, leading automation into the **Transaction Processing** flowchart.

In the **Main** workflow, let's replace the **Transaction Processing** flowchart with a sequence named **Classify Email**. Your screen should now look like this:

![](bbox=[0.222, 0.445, 0.781, 0.878])

Figure 9.22 – Main workflow