
Your diagram could look like the following:

| Section | Column 1 | Column 2 | Column 3 | Column 4 | Column 5 | Column 6 |
|---|---|---|---|---|---|---|
| **Environment** | SI team 1: Development environment (Developer sandbox/Scratch org)<br>SI team 2: Development environment (Developer sandbox/Scratch org)<br>SI team 3: Development environment (Developer sandbox/Scratch org) | SI team 1: SI QA environment (Developer or Developer Pro sandbox/Scratch org)<br>SI team 2: SI QA environment (Developer Pro or Partial Copy sandbox/Scratch org)<br>SI team 3: SI QA environment (Developer Pro or Partial Copy sandbox/Scratch org) | Optional: Hot-fix environment (Developer sandbox) | Optional: Training environment (Partial Copy sandbox) | Optional: Integrated testing environment (Developer Pro or Partial Copy sandbox) | Created from SCM branch: Integrated testing. This environment usually comes after the QA environment. |
| **SCM branch** | Feature branch | Feature QA branch | CI branch | QA branch | Staging branch | Version branch Main branch |
| **Activities taking place** | Develop new feature | Locally test a new feature | Merge new/changed metadata | QA tests | UAT tests, staging, and training | Production environment, features release to users |
| **Tests executed** | • Developer tests<br>• Unit tests | • Developer tests<br>• Unit tests<br>• Automated functional tests<br>• Manual functional tests | • Unit tests<br>• Automated functional tests<br>• Code quality check | • Unit tests<br>• Automated functional tests<br>• Manual functional tests<br>• Regression tests | • Unit tests<br>• Automated functional tests<br>• User acceptance tests<br>• Load tests<br>• Performance tests | • Unit tests<br>• Smoke tests |
| **Actors** | Developer | Developer | • Release manager<br>• Development team lead<br>• Developer | Release manager | Release manager | Release manager |

## Figure 13.6 – Development life cycle diagram (final)

You would also need an automated build and deployment tool. There are open-source solutions such as Jenkins and tools built specifically for Salesforce environments such as Copado and AutoRABIT. Select one and add it to your landscape. Also, remember to add an automated testing tool such as Selenium or Provar.

Move on to the next requirement, which begins with the following:

*Historically, SIs used to follow their own coding standards while dealing with other platforms.*

This is an interesting requirement. The challenge here is related to the lack of standardization and communication across the different SI teams.

Would CI/CD solve this? Not really. It can improve things a bit with automated code scanning and controlled merging, but to significantly improve standardization, communication, reuse, and overall quality, you need to introduce a Salesforce CoE.

Bringing together functional and technical resources, you can even introduce a design authority as another organizational structure to ensure every single requirement is discussed in detail, validated, and approved before starting any development efforts.

In Chapter 10, Development Life Cycle and Deployment Planning, you learned about the value and structure of each of these. Figure 10.3 (Proposed CoE structure) and Figure 10.4 (Proposed DA structure) can also be handy to visualize the two structures.

You are unlikely to be asked to draw the CoE or DA structures, but most likely you will be asked about the value and the challenges they address. If you are already on a project that has both, that is excellent news. Otherwise, do not be shy about proposing this to your stakeholders. This structure brings tangible benefits to the project and, at the same time, helps develop the skills of its members. It is a win-win for everyone.

*PPA has a new platform owner who joined from another company that also used Salesforce.*

Based on her experience from her previous workplace, the newly joined platform owner reported that bugs were fixed in UAT on several occasions but then showed up again in production. Such a sad experience impacts many people’s judgment of the platform’s agility and ability to adopt modern release management concepts. Usually, it ends up being a bad adaptation of a sound concept. On other occasions, such as this one, apparently, it is due to a poor environment management strategy and release cycle.

When a bug is fixed in UAT, UAT’s code base becomes unaligned with the code base in lower environments. The next time a feature is promoted from lower-level environments (such as development environments), it could easily overwrite the changes made previously in UAT. Eventually, when this is pushed to production, the bugs that were resolved come back.

You could simply say that this symptom is happening because the client did not use a proper CI/CD release management strategy and probably relied on mechanisms such as change sets. However, this is not considered a good enough answer. Instead of using buzzwords, you are expected to explain, educate, and justify in detail. This is how a CTA should respond during the review board and in real life.

This issue could be resolved in one of three ways:

- After fixing the UAT bug, every lower environment should refresh its code base to get this new change. This requires a high level of discipline and organization.
- Upon promoting the lower environment's code, avoid overwriting existing and conflicting features in UAT. Attempt to resolve that with the development team. This is not always easy and can become difficult when dealing with point-and-click features such as Salesforce Flows.
- Fix the bug on a hotfix branch and environment, merge the branch into UAT, then promote it to production. The hotfix branch is also used to refresh all lower-level environment branches. This option sounds very similar to the first, but the difference is that it is more trackable and easier to govern. You still need an adequate level of discipline and organization.

Release management tools such as Copado make this process easier than usual using the Graphical User Interface (GUI) and point-and-click features rather than a pure Command-Line Interface (CLI).

You must answer such requirements with enough details, but do not get carried away. Keep an eye on the timer and ensure you leave enough time to cover other topics, whether this is taking place during the presentation or Q&A.

Move on to the final requirement in this section, which starts with the following:

*PPA would like to release the solution in Italy first, then roll the same solution out in other countries based on a pre-agreed schedule.*

You have already explained the value of the CoE structure in these situations. It is the best organizational structure to ensure everyone is involved, sharing their ideas and feedback, and aware of the planned features and roadmap. This is where conflicting business requirements are detected and resolved. The CoE will aim to create a unified process across all countries.

However, in addition to that, you need the right delivery methodology to ensure the smooth rollout of the solution. The CoE and DA will ensure that requirements are gathered, validated, and solutioned in the right way to guarantee high quality. Moreover, they will also ensure that the features are developed to accommodate the slight process variation between countries. A 100% unified global process is a hard target to aim for. Your solution should be designed to accommodate and embrace slight variations in a scalable manner.

Consider the following when choosing the right delivery methodology:

- A rigid **waterfall methodology** will ensure a clear scope with a detailed end-to-end solution. However, it will also drive a great distance between the development team and the business that would eventually use the solution. Moreover, it has a longer value realization curve.

- An **agile methodology** ensures that the business is closer to the developed solution. This increases adoption and enables quick feedback. However, without a clear vision, this could easily create a solution that is a perfect fit for one country but unfit for others. This would be difficult to maintain even in the short term.

- A **hybrid methodology** ensures you get the best of both worlds with minor compromises. You get a relatively clear scope during the blueprinting phase with an end-to-end solution covering 70%-80% of the scope. It is not as rigid and well defined as you would get in a waterfall methodology, but it mitigates many risks from day one.

The solution gets further clarified during the sprints’ refinement activities. With the DA structure in place, you ensure that every user story’s solution is challenged and technically validated by the right people before you start delivering it.

Again, keep an eye on the timer while explaining such requirements. Some candidates get carried away while explaining this part, mostly describing personal experiences. Also, remember that you have very limited time. Your primary goal is to pass the review board.

Many candidates run out of steam at this stage of the presentation, which is totally understandable considering the mental and emotional efforts required up to this point. However, you need to keep your composure. You are close to becoming a CTA. Do not lose your grip on the steering wheel just yet.

That concludes the requirements of the project development section. The next section is usually limited and contains other general topics and challenges. Maintain your momentum and continue to the next set of requirements.

## Analyzing the Other Requirements

The other requirements section in a full mock scenario could contain further requirements about anything else that has not yet been covered by the other sections. There is usually a limited number of requirements in this section.

Your experience and broad knowledge of the Salesforce Platform and its ecosystem will play a vital role at this stage. Next, explore the shared requirement.

_PPA wants to ensure that data in development environments is always anonymized._
