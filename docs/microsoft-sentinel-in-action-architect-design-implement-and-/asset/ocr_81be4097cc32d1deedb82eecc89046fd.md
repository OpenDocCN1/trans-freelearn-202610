In this step, you will determine how you can perform continuous monitoring to help safeguard against the situation you were investigating or improve the ability to investigate again next time. Can you add more information to the log? Is there a change in the operating procedures that can be performed to help avoid this situation in the future? Is there a change to your network that can be performed?

One of the best outcomes would be to create an analytic query so that this situation can be found automatically and perhaps handled via a playbook. Refer to Chapter 12, Creating Playbooks and Automation, for more information on creating playbooks.

# Improving

Based on any learnings that came from the investigation, improve the investigation code and techniques, if possible.

In this step, you – and, most likely, others – will work to determine how to improve the queries used in this hunt, as well as how to avoid needing this hunt in the future. It may be that the queries you used could be rewritten to be more efficient, changes could be made to operating procedures to avoid the situation altogether, or other recommendations may be made based on the skills and experience of your team, partners, and domain-specific experts.

Those are the steps that can be taken to start your threat-hunting investigation. Again, none of these are set in stone, and the steps are only provided for guidance. You may find that your company has its own steps or that you want to add your own. The real takeaway from this is that you should have a repeatable process that is always improving, enabling you to share your results at least with your co-workers and with the threat-hunting community at large, if you can.

# Summary

Now, you have learned about how to start doing threat hunting in Microsoft Sentinel. You learned about the Hunting page, the Notebooks page with its Jupyter notebooks, and got a brief introduction on how to perform a threat-hunting investigation.

We looked at the tools that Microsoft Sentinel provides to assist with threat hunting. This includes queries that only get run periodically, either due to factors such as needing to look for a specific piece of information or the fact that they would return too many results to be useful on a scheduled basis.