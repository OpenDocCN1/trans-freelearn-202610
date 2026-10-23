# Windows machines

+   - [ ] Select all
+   - [x] Critical updates
+   - [x] Security updates
+   - [ ] Update rollups
+   - [ ] Feature packs
+   - [ ] Service packs
+   - [ ] Definition updates
+   - [ ] Tools
+   - [ ] Updates

Figure 10.11– Define update classifications

A best practice here is to have different *maintenance configurations* related to different parts of your infrastructure and updates. One example would be the following:

+   - **Defender Antivirus definition files:** Scheduled daily
+   - **Security and critical updates:** Scheduled on the second Tuesday of each month
+   - **Custom configurations:** These are for high-severity security updates, where we add the specific **Knowledge Base (KB)** ID to install those updates as soon as possible

> **Note:**
> A Windows **KB ID** is a unique identifier assigned by Microsoft to each individual knowledge base article or security update. The KB ID is a combination of letters and numbers, and it is used to easily identify and reference a specific update or article. The KB ID is included in the title of the article or update, and it is also used when downloading or installing updates via Windows Update or the Microsoft Update Catalog. The KB ID is also used when searching the Microsoft website or support pages for information about a specific update or issue. It's a unique identifier that can be used to track the specific patch, update, or security bulletin.

This is, of course, one way to handle updates of virtual infrastructure across public cloud and on-premises infrastructure. Microsoft also has other features, such as **Automanage**, which provides some of the same capabilities that manage backup, configure monitoring, and enable automatic updating, but then you leave update management to Microsoft directly.