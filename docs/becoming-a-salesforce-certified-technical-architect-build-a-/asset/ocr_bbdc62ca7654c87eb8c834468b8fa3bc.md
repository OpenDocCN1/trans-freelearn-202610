However, this approach has the following cons:

+   - You need to keep any regulations that could prevent you from selecting this approach in mind.
+   - Since you might be accommodating several business units with different processes and requirements, the single org could grow quickly in terms of complexity. This means it might end up becoming very difficult to maintain.
+   - You have a higher chance of hitting the governor’s limits.
+   - Due to the org’s complexity and the fact that changes could impact many users and business units, innovations and changes could be slow and limited.
+   - The advantage you get in terms of maintaining all policies, profiles, and permissions in a single place could also be a disadvantage when these configurations grow too complex to maintain.
+   - Different teams could be working on conflicting features, which might end up breaking each other. A thorough regression test strategy is key in such an environment. A regression test is a type of software testing that checks and validates whether the recent software changes had adverse effects on any of the existing features.
+   - It is difficult to administer locally.

# Multi-org

In this approach, your solution is delivered using multiple Salesforce production orgs. They can be completely autonomous or designed to be in a master-child architectural style where a master org pushes or pulls data to/from multiple child orgs or is designed without a central org, where some of the orgs are connected directly to each other to exchange data. This approach has the following pros:

+   - Fewer chances of hitting the governor’s limits.
+   - Innovation and time to market are better than the single-org approach since changes that are made to each org do not impact the other orgs.
+   - Fewer chances of conflicts between different business units as they can work on their own orgs.
+   - Less complexity in general in each org.
+   - Easy to administrate locally.
+   - A better ability to handle different regulatory requirements.
+   - Higher flexibility with release cycles since each org can have its own.