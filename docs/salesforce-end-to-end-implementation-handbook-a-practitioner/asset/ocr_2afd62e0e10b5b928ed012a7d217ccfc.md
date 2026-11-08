To learn more about AI and Salesforce, you can read *Architecting AI Solutions on Salesforce* by Lars Malmqvist (CTA) ([https://www.amazon.com/Architecting-Solutions-Salesforce-state-art/dp/1801076014/](https://www.amazon.com/Architecting-Solutions-Salesforce-state-art/dp/1801076014/)).

In addition to analytics and automation use cases *within* your Salesforce org, let’s consider use cases that integrate and automate processes with *external* systems:

+   - **MuleSoft RPA**: Using MuleSoft to, amongst other things, use **Optical Character Recognition (OCR)** to process received documents and transform these into system-consumable data:
+     - Use cases may include receiving an order by PDF, and instead of a human manually creating the order, MuleSoft can do it for you.
+   - **MuleSoft Composer**: A declarative (no-code) solution enabling admins and functional consultants to set up cross-system process automation:
+     - Although Composer is a declarative solution, make sure you follow the same design governance rigor as you would for any integration – require solution designs to be reviewed by your **Design Authority**.
+     - Use cases include posting case updates to a Slack channel. A couple of things to consider are as follows:
+       - Composer is designed for low-volume processes, not batch or bulk use cases.
+       - There is no concept of sandboxes for testing, so be diligent when “turning on integrations” by connecting Composer flows to production.