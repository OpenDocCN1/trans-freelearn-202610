Now, have a quick look at the details of CI:

+   - CI activities include continuously and frequently merging the developers’ work into a shared build environment. It is preferable for this to be automated. But what is the value of that?

The value is mainly in preventing conflicts and detecting them as early as possible. Developers work in different environments (except if they use a shared development environment, something you should try to avoid as it makes it difficult to trace the developed functionalities and tie them back to requirements). If they are aware of the changes that have been made by other developers, there will be fewer chances for conflicts.

Merging their work regularly and frequently (some practices recommend doing this multiple times a day) ensures that they are aware of the conflicts they have with others as early as possible. This work is typically merged into a specific source control’s build branch associated with the build environments.

+   - CI also involves ensuring that the developers are preventing conflicts by working on the latest code base (as much as possible). If the developer is working on a 10-day-old code base, it is very likely that another developer has made significant changes to the platform during that period. The chances of that become much lower if the code base is only a few hours old.

To do that, the developers are expected to fetch the latest code base, which is usually available in the build branch. This could take place in multiple shapes and forms. It could be manual, or automated using a simple script, or even using an automation tool. An alternate approach is to build the entire developer sandbox using the code base, which is available either in the source control branch or in the build environment itself.

Scratch orgs are a good example of the first case, where a new scratch org is created and built using the metadata stored in source control every time a developer delivers a new feature.

Sandbox cloning is a good example of the second case: you can clone the build sandbox to create a new development environment with the latest code base.

You need to understand CI and the rationale behind it. It is recommended that you practice it if possible.

CI comes with its counterpart, **continuous delivery** (CD). CD is more straightforward; it complements CI and offers help to bridge the gap between development and operation by automating an application’s testing and deployment. Establishing the right multi-layered development environment strategy, selecting the right release management automation tools, and crafting a comprehensive automated test strategy will ensure that you have the right building blocks for CD.

The last element to cover here, which is a crucial part of your CI/CD strategy, is source control. **Source control management (SCM)** (also known as version control) provides you with the ability to track and manage code changes easily and efficiently. It helps you detect and resolve conflicts, and it provides you with a mechanism to roll back changes to an earlier version if needed. **GitHub**, **Bitbucket**, and **GitLab** are very popular SCM solutions.