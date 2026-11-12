
# Capital One's Cloud Transformation and Mobile App Re-architecture

## Upskilling IT Workforce into Cloud Experts

In this section, we will briefly explore how Capital One upskilled its talented IT workforce into cloud subject matter experts who were predominantly experts with on-premises technology. Seeking change, the leadership at Capital One created a unicorn job specification and hoped to hire top cloud talent from the market. This just didn’t work because that unicorn just didn’t exist. Capital One quickly realized that it could transform the extremely skilled, proactive, and dedicated talent they already had. In hindsight, Capital One articulates the steps that it put in place that worked wonders for them *[25]*:

1.  **Acceptance**: This was a crucial first step where Capital One had to convince its talented engineers and leaders that they could learn modern cloud skills – in this case, AWS – and be able to upskill. Instilling the right level of confidence, creating a learning environment, laying out a clear path, giving ample time, and incentivizing were just enough.

2.  **Training**: The team at Capital One swiftly started offering learning opportunities to its team through official classroom-led training.

3.  **Getting their hands dirty**: Change is extremely hard and goes against human psychology. However, creating the right environment with blameless outcomes can be a powerful change agent. Capital One offered sandbox environments for its engineers to play around in the cloud helping create a solid learning environment.

4.  **The two-pizza team**: The first engineering team that Capital One put together considered a good mix of IT folks from different expert areas, such as networking, database, system admins, application engineers, storage admins, and security engineers. This made perfect sense since cloud technology blurs the lines between these job roles, which are typically distinctly seen in a traditional on-premises set up.

5.  **Cross-pollinate with external expertise**: It was essential to help cross-pollinate the home IT with some external coaching and hand-holding. This is where partnerships can bring a significant edge. Capital One brought in experts from AWS partners who were professionally certified to help with the initial rollouts. This had a transformative effect, where the Capital One team learned from watching, asking questions, and doing it themselves, getting them ready for the future.

6.  **Making it real**: Setting a goal for the two-pizza team to build and deliver something real and in production was a key move. The objective here was to find a workload that is important but not too mission-critical. The key here was to ensure this identified workload didn’t require too much customization of the cloud building blocks (in this case, AWS), as it may serve as a technical roadblock to start with.

7.  **Scale learning with cellular mitosis**: Once the first team delivered a successful outcome, it was time to split this team – who now have the necessary experience – into two four-member teams while introducing four more engineers into each team. This helped scale the learning through hands-on and learning on the job. Capital One repeated this process until it had hundreds of skilled cloud engineers with the added side effect of a bunch of net new cloud applications.

8.  **Cloud certification**: Capital One saw a direct correlation between engineer certification and the number of applications delivered on AWS. The certification process ensured that the engineers at Capital One understood the AWS language and had a broader understanding of the technology.

9.  **Scaling certification and leadership**: Capital One advocates for a critical mass of 10% of the engineers to be certified and advocate for the platform after which the network effect takes hold. Grooming and motivating engineers to get certified resulted in creating a halo effect that also helped Capital One’s perception not just internally but also externally.

10. **Recognize and reward**: The cloud (AWS) certification at Capital One was viewed as a great achievement. Recognizing and rewarding engineers publicly had a huge effect on team morale and motivation.

11. **Challenge yourself**: As a leader, it was also important to practice the preachings. At Capital One, leadership stepped up and took the certification challenge to have a broader and deeper understanding of the AWS lexicon.

12. **Redefining job families**: The Cloud had a far-reaching impact and is a transformation not just in the technology space but also across business functions, processes, and operations. Capital One redefined the job portfolio to reflect the same.

This holistic strategy helped Capital One not only acquire the right talent from the market but also harness its in-house talent by upskilling for cloud transformation. Now, let’s explore how Capital One envisioned and pioneered creative mobile experiences for its consumers.

## Re-architecting the Capital One mobile app on the cloud

The pinnacle of Capital One’s cloud transformation effort was the re-architecture of its largest customer-facing mobile app. This was an ambitious project as this mobile app had a user base of tens of millions of customers. This was much more than a simple lift and shift and required a full re-architecture of the entire application, including the integration, orchestration systems, and operational support tooling. Building cloud-native software can often be a whole new ballgame as it requires a shift in mindset from traditional software development to cloud-native DevSecOps *[26]*.

The mission was simple: Capital One Mobile must be capable of keeping up with consumer demand, backed by an infrastructure that moves with the needs of the product.

One major advantage Capital One had was that the majority of the mobile application’s existing architecture was “cloud-ready.” For instance, the application leveraged microservices-based API-driven architecture (Capital One refers to this as micro experience APIs – more on this later), Netflix Zuul OSS for server-side load balancing, Netflix Hystrix OSS for service fault tolerance, Netflix Eureka as a service registry, and Ansible for operational automation. This meant that the team at Capital One only had to make minor adjustments to ensure AWS’s cloud-native capabilities were fully leveraged *[27]*. For instance, from deploying service components within a single contained infrastructure on-premises, better scale and fault-tolerance can be achieved by deploying them on distributed infrastructure in AWS.

Capital One coined the concept of **Experience APIs** (**xAPIs**) and **Micro Experience**. xAPIs differ from regular APIs in that xAPIs focus on delivering a user experience flow, while a regular API can refer to servicing a resource or data model. Micro experience is essentially a collection of multiple xAPIs that can be put in a single deployable unit. For instance, a customer clicking on *Redeem Rewards* can be a micro experience that can comprise multiple micro experience APIs.

![Figure 7.11 – Capital One’s mobile application architecture](image/B18893_07_11.jpg)

Figure 7.11 – Capital One’s mobile application architecture

Having explored Capital One’s mobile application architecture, let’s review the challenges and learnings from their early cloud DevOps efforts.

### DevOps challenges and learnings

The following are some of the Cloud DevOps best practices that we can glean from Capital One’s major cloud transformation effort:

*   Leverage cloud-native capabilities wherever possible to improve efficiencies. For instance, Capital One replaced the corporate GTM solution with AWS Route53 and migrated data stores from on-premises RDBMS to RDS.

*   Develop a deployment pattern for service-based deployment instead of a full stack deployment to improve the frequency of micro releases and incremental deployments.
