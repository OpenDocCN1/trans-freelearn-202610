
Coming back to the company’s requirement, this can be met by adopting an MVP style of product design and development. In your presentation, you need to briefly explain what an MVP solution is, the value it brings, and how you can ensure it meets its future potential. Move on to the next requirement.

The team maintaining the ERPs works in a six-month release cycle, and they are unable to modify their timeline to suit this project.

This could potentially be a risk. You need to bring up any foreseen risks during your presentation. Considering the ERP’s release cycle, you might encounter situations where a Salesforce release is held back until the next ERP release. This means there will be a need to carefully manage and select the released Salesforce features in any version.

Moreover, you should have the right environment structure to control feature developments and the right integration strategy to enable loose coupling.

The proposed environment structure and development life cycle strategy should cater to these requirements and clearly explain how they would address this risk. You will cover this in the presentation pitch later in this chapter.

Historically, the customer support team is used to high-productivity systems, and they have a regulatory requirement to handle calls in no more than 10 minutes.

You can improve the productivity of the customer support team using the **Service Cloud Console** (also known as **Lightning Service Console**). The console allows configuring some shortcuts as well to meet the company’s expectations. Also, **Service Cloud Voice** allows automatic contact lookup based on the caller’s phone number. **Salesforce** macros can also be used to speed up some repetitive tasks (such as sending an email to the customer and updating the case record with a single click).

All these features will further improve the productivity and response time of the customer support team.

The company would like to get support in identifying potential project risks.

You have already come across three potential risks:

- Designing and governing the MVP solution
- Aligning the relatively slow release cycle of the ERPs with the quick-release cycle adopted by Salesforce
- Managing the user expectations and performance while switching to a cloud-/browser-based solution compared to a high-productivity desktop application

You need to call each of these out during the presentation and clearly explain how you plan to mitigate each. You will do that later in this chapter.

*The company would like to have a clear, traceable way to track features developed throughout the project life cycle.*

In Chapter 10, Development Life Cycle and Deployment Planning, you came across several techniques to govern a release cycle, including tracking developed features.

You have also learned that the best mechanism to handle such situations in Salesforce is by implementing source code-driven development using a multi-layered development environment.

**Source code-driven development** will allow you to maintain a trackable audit trail that shows the developed features at any given time of your project life cycle. This requirement is also related to the next. Go through that requirement to help you formulate a comprehensive response for both. The next requirement is as follows:

*The company is looking for recommendations for the right environment management strategy to ensure the proper tests are executed at each stage.*

This requirement could be asked in several different ways. The answer will always lead to what you learned in Chapter 10, Development Life Cycle and Deployment Planning. The multi-layered development environment, automated release management, and test automation tools and techniques should be used to fulfill this requirement.

You saw a sample development life cycle diagram in Chapter 10, Development Life Cycle and Deployment Planning Figure 10.7 (Development cycle life cycle diagram (final)), which you used as the base for the proposed environment management and development life cycle diagram in Chapter 13, Present and Defend: First Mock. You will use the same diagram as the base for this scenario too. You can assume one development stream in this case. Your diagram could look like the following:

| Environment | Development environment (Developer sandbox/ Scratch org) | QA environment (Developer or Developer Pro sandbox/ Scratch org) | CI environment (Developer sandbox) | QA environment (Developer Pro or Partial Copy sandbox) | Staging environment (Full Copy sandbox) | Production |
|---|---|---|---|---|---|---|
| SCM branch | Feature branch | Feature QA branch | CI branch | QA branch | Staging branch | Version branch Main branch |
| Activities taking place | Develop new feature | Locally test a new feature | Merge new/changed metadata | QA tests | UAT tests, staging, and training | Production environment, features release to users |
| Tests executed | Developer tests Unit tests | Developer tests Unit tests Unit tests Automated functional tests Manual functional tests | Unit tests Automated functional tests Code quality check | Unit tests Automated functional tests Manual functional tests Regression tests | Unit tests Automated functional Tests User acceptance tests Load tests Performance tests | Unit tests Smoke tests |
| Actors | Developer | Developer | Release manager Development team lead Developer | Release manager | Release manager | Release manager |

Figure 15.6 – Development life cycle diagram (final)

You will notice that you still have a CI sandbox (also known as the build sandbox) and QA sandboxes before and after the CI environment. You might wonder why you need them. After all, there is only one development stream, and things will be more straightforward with fewer environments, right? Not exactly.

Your release strategy should be future embracing. It should never be short-sighted to only cover near-future requirements. Your strategy should be able to extend and accommodate multiple project streams developing new/updated features concurrently, even if this is not currently an urgent requirement.

In most Salesforce projects, there will be at least two concurrent streams at any given time. There would be a stream building and developing new features and a stream maintaining the production org and fixing and patching any encountered bugs. All code changes should ideally be merged into the CI environment to follow the designed pipeline (apart from some exceptional cases). Your environment strategy should be able to cater to that and more.

Local QA environments help the development team test their functionalities before they can progress any further. Shared QA environments are typically managed by dedicated testing teams who ensure that everything fits together and works as expected.

Things will indeed get more complicated with additional environments, except if you have the right tools in place to support you. This is why source control management (SCM) and automated build and release management tools are crucial for any enterprise-scale Salesforce project's success.

For the company's scenario, you need to suggest an automated build and release tool. You can propose a tool such as Copado. You also need to add an automated testing tool such as Provar. Keep in mind that you still need to explain how your unit tests would work side by side with Provar to provide a comprehensive testing experience. You will cover that later in this chapter while crafting the presentation pitch. Add Copado and Provar to your landscape diagram and move on to the next and final requirement.

The company is looking for an appropriate methodology to manage the project delivery and ensure proper technical governance.

This is a combined requirement that you can address using the knowledge you gained regarding the various developing methodologies and the different governance bodies you came to know about in Chapter 10, Development Life Cycle and Deployment Planning.

You need a delivery methodology capable of supporting the MVP solution, that is, a methodology that caters to a blueprinting phase to plan the program's outcome and define how that would be achieved, preferably with some proofs of concept. However, the methodology should also be flexible enough to accommodate and embrace other requirements beyond the MVP phase. All these should point you to the hybrid methodology.

To ensure technical governance, you need a governing body capable of bringing together the right people at the right time to make decisions, empowered by executive sponsorship and supported by expertise from around the enterprise. This is precisely what the CoE is for.
