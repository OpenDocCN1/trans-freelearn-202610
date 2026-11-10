Applications and APIs also make data useful by operating on it, updating it, and connecting it across different systems. Protecting these systems (or applications) as an individual discrete asset (rather than as part of a network) allows us to be more agile. For example, we can quickly allow a partner to access a single system or API rather than sharing access to all our internal systems on a network-wide basis. While this may seem like common sense, this is not possible with classic architectures and represents a fundamental shift to enable business agility with Zero Trust.

# Security zones

Security zones are groupings of similar assets with similar security requirements. Grouping assets this way allows us to focus resources based on how much protection is needed, spending the most time and effort on the most valuable assets (aka the *crown jewels*) rather than on the cafeteria menu. This concept is not new to security but is updated with Zero Trust.

Security zones also provide agility to the business by making business decisions simpler. Instead of having to work through a long, confusing list of technical assets for every business process, you can simply reference the zone name.

You can think about security zones like physical spaces in a retail bank:

+   - Outside the bank is fully public and there is limited control of the physical space, but you can install lights and cameras to increase the visibility and capture a record of suspicious behavior
+   - The bank lobby is open to the public but is a much more controlled space (via door locks and so on) and allows for even more monitoring
+   - Behind the counter is a restricted employee-only area where social and physical barriers (up to and including bulletproof glass) can protect employees and money
+   - In the vault is a highly guarded and protected zone where only select employees are allowed to interact with the highest-value items (and the highest density of valuable items)

Classic computer security often focused on trying to secure the whole bank building as if it were a vault. This attempt at simplification avoided identifying and analyzing individual business assets and their many (often undocumented) interdependencies but was impractical and often increased security risk. Everything was in this single vault (sometimes called a *flat network*), so all employees and partners had to be let into it, meaning any single mistake put everything in the vault at risk.

The strategy in the playbooks focuses on a security zone strategy that builds a general baseline for all assets and then establishes higher-level security zones for the highest-impact assets (aka the *crown jewels*).