
I have identified at least three potential risks in this project. Firstly, the design and governance of the MVP solution. To mitigate this, we will use a governance body consisting of two parts: a Center of Excellence (CoE) to ensure the strategic decisions are taken with the involvement of all stakeholders and a Design Authority (DA) responsible for tackling the daily project challenges and coming up with a justified and auditable solution based on best practices.

Next is aligning the relatively slow-release cycle of the ERPs with the quick-release cycle adopted by Salesforce. We will use a multi-layered development environment and integrate it with the right ERP environment to mitigate this. We will establish a loosely coupled integration between the systems using MuleSoft, which will allow us to utilize a two-speed architecture concept. The two-speed architecture will allow the integrated systems to develop at their own pace with minimal dependencies in between. A transparent backlog across the project will ensure everyone is aware of the features being developed. The CoE will ensure the right features are selected for the correct release.

The final risk is managing the user expectations and performance while switching to a cloud-/browser-based solution compared to a high-productivity desktop application. To mitigate this, it is important to ensure that we have the right change management process in place, including any communication campaigns, training, the definition of trailblazers/superusers, and continuous support from the executive team. We will use tools that closely match the desktop application’s performance.

We will use source code-driven development using a multi-layered development environment. Source code-driven development will allow you to maintain a trackable audit trail that shows the developed features at any given time of your project life cycle.

In this diagram [point to the development life cycle diagram], you can see that we will initially have one development stream.

The team in this stream will be using a development sandbox to develop new features. They will use another developer sandbox to test the developed features without disruption from any development activities. There will be a sandbox used for the continuous merging of newly developed features from all streams.

On top of that, there will be a QA environment sandbox managed by the testing team, a staging environment, and a production environment. Hotfixes can take place on a separate sandbox and then get deployed into CI, staging, and production. This setup will allow future streams to work in parallel and ensure that all teams are always using the latest code base and avoiding conflicts.

I propose using automated deployment and release management tools such as Copado and an automated test management suite such as Provar. An Apex unit test will still be used to test all critical functionalities, in addition to functionalities that cannot be tested using a UI-based automated test.

Each time a functionality is deployed from one environment to another, all Apex unit tests will be invoked. Automated test scripts will also be automatically invoked. Together, they will ensure that all regression tests are executed and that newly developed features meet all the acceptance criteria. There will still be manual tests, of course, but we should aim to automate the most critical use cases.

The judges will most probably ask questions related to this domain in the Q&A stage, such as the structure of a CoE, the proposed development methodology, the rationale behind the CI environment, the use of branches in the SCM, how to keep all development environments up to date, and how to ensure the tests are consistently being invoked.

You have seeded some questions here as well. For example, you have briefly mentioned that the CI setup will allow teams to avoid conflict, but you did not explain precisely how. You will tackle that question later in this chapter. Move on to the catch-all phase of the presentation.

Going through the scenario and catching all the remaining requirements.

At this part of the presentation, you will go through the requirements one by one and ensure you have covered each aspect.

You learned about this approach's benefits in Chapter 13, Present and Defend: First Mock, where it helped you spot and solve a missed requirement. Even if you have created a perfectly structured presentation, it is recommended to leave some room to go through the requirements again and ensure you have not missed any.

If you are doing an in-person review board, you will receive a printed version of the scenario. You will also have access to a set of pens and markers. When you read and solve the scenario, highlight or underline the requirements. If you have a solution in mind, write it next to the requirement. This will help you during the catch-all part of the scenario.

If you are doing a virtual review board, you will get access to a digital version of the scenario. You have access to MS PowerPoint and Excel, and you can copy the requirements to your slides/sheets and then use that during the presentation.

That concludes the presentation. The next part is the Q&A, the stage where the judges will ask questions to fill in any gaps that you have missed or not clearly explained during the presentation.

## Justifying and Defending Your Solution

Next, move on to the Q&A, the stage that should be your friend. Relax and be proud of what you have achieved so far. You are very close to achieving your dream.

During the Q&A, you should also expect that the judges will change some requirements to test your ability to solve on the fly. This is all part of the exam. Next, explore some example questions and answers.

The judges might decide to challenge one of your proposed solutions. This does not necessarily mean that it is wrong. Even if the tone suggests so, trust in yourself and your solution. Defend, but do not over-defend. If you figure out that you have made a mistake, accept it humbly and rectify your solution. The judges will appreciate that.

For example, a judge might ask the following question:

> You proposed introducing an integration interface to pull the smart meter readings from the devices in your solution. This means that we are unable to get a real-time reading of the meters where possible. Why have you proposed this solution, and is there any alternate way that can give us real-time data?

While answering such a question, make sure your answer is sharp and to the point. Do not try to muddy the water and hope to slip away. Your answer could be as follows:

> The question indicated that only two systems could push the meter readings to the server (via pub/sub). Moreover, the requirement was to read these meters once a month. PDU was keen on developing a unified mechanism to retrieve the smart meter data from all its vendors.

> Therefore, I thought of developing a scheduled monthly MuleSoft job in MuleSoft that pulls the meter readings from all these platforms, then updates Salesforce with the readings.

> Alternatively, we could investigate whether to introduce a pub/sub layer on top of the two systems that do not support it. This could be achieved by connecting MuleSoft to their database and periodically checking for changes, assuming that these systems can provide a mechanism to retrieve the delta of changed data, something similar to the CDC concept.

> If these systems do not support this capability, we could consider integrating with these systems in two different ways. The first would be the unified pull interface, while the second could use MuleSoft to subscribe to the pub/sub channels provided by the two systems. This way, we get the updates from two out of four systems in real time. This will be against the company's original requirements, though.

> I favor the pulling mechanism because it ensures we do not update Salesforce records unnecessarily. This will ensure we adhere to the governance limits and avoid any performance challenges.

Your answer should take no more than three minutes. Remember that you have a limited Q&A time, and you must ensure you leave room for the judges to ask as many questions as possible. The Q&A phase is your friend.
