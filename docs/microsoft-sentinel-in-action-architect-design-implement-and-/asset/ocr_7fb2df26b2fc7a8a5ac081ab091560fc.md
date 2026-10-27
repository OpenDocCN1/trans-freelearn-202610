As shown in the following screenshot, there are different actions that you can perform:

![](bbox=[0.405, 0.12, 0.6, 0.31])

Figure 12.8 – Automation rule actions

Each of the available actions is described in the following table:

| Action | Description |
|---|---|
| Run playbook | This will allow you to select which playbook you can run. Only those playbooks using the incident trigger will be shown. Refer to the Types of playbooks section in this chapter for more information. |
| Change status | This will automatically change the incident's status. If selected, another dropdown will be shown where you can choose its status. |
| Change severity | This will automatically change the incident's severity. If selected, another dropdown will be shown where you can choose its severity. |
| Assign owner | This will automatically change the incident's owner. If selected, another dropdown will be shown where you can choose the owner. |
| Add tags | This will automatically add the tag(s) to the incident. If selected, a new section where the tags can be entered will be shown. |

Table 12.2

You can have multiple actions in the same rule. If there is more than one action, the first one will run, then the second one will be started after the first, and so on.

The Rule expiration field allows you to select how long this rule will be active. By default, it will be Indefinite, in which case the rule never stops being active. You can select a date and time when the rule will expire, in which case the rule will be deactivated when it reaches that date and time.