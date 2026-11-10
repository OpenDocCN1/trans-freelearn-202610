
- **DevOps becomes DevSecOps**: Adding security to the DevOps process of creating and updating applications makes it DevSecOps. DevSecOps lowers cost and security friction because more security issues are found and fixed early in the process when it's cheap and easy to fix them (often called *shift left*). This requires teams that create and own assets to integrate security practices early in the development process. DevSecOps helps mitigate supply chain security risks such as those that affected SolarWinds in 2020.

## What are DevOps and DevSecOps?

The classic IT model has separate teams for software **Dev**elopment and IT operations (**Ops**). Many organizations are on the journey to adopting a modern DevOps approach that fuses these into a single organizational function. Adding **Sec**urity makes it into **DevSecOps**.

*Integration is key to operations success—a siloed approach to operations will not work. In future chapters, we take a deeper dive into operational dependencies, which roles are involved, and what each role does to enable the integration.*

## Where is “security management”?

The term *security management* (or information security management) is sometimes used to describe multiple operational and operating model disciplines that have a close relationship with **IT service management** (**ITSM**). We chose to break security management down into its core capabilities to increase clarity and actionability.

Security management is addressed by Zero Trust with the following:

- **Security governance** sets the security policy and standards in partnership with operational teams. This discipline also supports compliance-reporting activities for external requirements and internal policies and standards, includes security architecture, and has an emerging responsibility to collate and distribute strategic TI (high-level summaries of security threats) to stakeholders.
- **System asset protection** applies security policy and standards to system assets. This is typically performed by the IT, DevOps, and business-line teams that operate and manage the assets.
- **Posture management** ensures the policy is being followed and enables asset protection teams to be successful. These teams monitor dashboards and engage with various teams to help them execute the policy, identify challenges to meet the policy, and provide feedback to governance teams. The feedback could include context on noncompliance trends, suggestions on improving policies, identification of gaps in tooling or education, and so on.

Now that we understand the operational pillar of Zero Trust, let's see how to make this pillar real for your organization and industry.

## *Tailoring is key to the operational pillar*

While a Zero Trust strategy can be similar across different organizations, the operational pillar must be adapted to your organization. We need to understand the organization and how it operates to integrate Zero Trust successfully. We need to understand the unique products, business processes, business functions, and organizational structure.

We need to understand who is involved and the business functions they support. The following are some examples of different operational models:

- A **healthcare payer** (insurance company) works with patients, providers (doctors and nurses), and so on, providing business functions such as claims processing and provider credentialing.
- A **manufacturer** works with distributors, part suppliers, and raw material suppliers and then provides the design and assembly of parts, finished goods, or equipment. Manufacturers rely on OT such as **industrial control systems (ICSs)** to control physical processes.
- A **financial institution** works with individual consumers, corporate clients, and government regulators in the context of offering loans, investment services, advisory services, and so on.

We have included many *Acme* examples throughout the series to help you tailor Zero Trust to your organization.

## On business functions and business processes

For reference, **business functions** are the big elements of the business, such as *production*, *claims processing (in the case of insurance companies)*, or *sales*. These functions can also be further broken down as needed, such as breaking sales into *corporate sales* and *small business sales*. Each business function is typically associated with one or more **organizational units (OUs)** and a set of **business processes**.

For example, *sales* might have business processes such as *lead generation* and *deal closing*. Business processes can also cross business functions and organizational boundaries to deliver business services (providing customer feedback, updating pricing, and so on). *Figure 8.9* illustrates how business processes span across multiple business functions:

![](img/c2bdb6ffe8b251139bc9db4439c8ed5a_163_0.png)

Figure 8.9 – Business functions and business processes

Now let's take a look at how to build the operational pillar.

## A process for building our operational pillar

> "Without strategy, execution is aimless. Without execution, strategy is useless."
> 
> – Morris Chang

This process shows how to integrate your Zero Trust strategy into your existing operational environment:

- 1. **Determine your product model** to align the Zero Trust roadmap to products (goods and/or services) that your organization produces and the way you produce them.
- 2. **Determine business functions**, including the organizational structure, **standard operating procedures (SOPs)**, and business processes in general. Business functions may sometimes be referred to as departments, divisions, units, or teams. While Zero Trust doesn't change what these are, it may impact the organizational structure and will probably modify business processes.
- 3. **Determine business processes** that run across the organizational structure and business functions to enable the production of goods and services. Zero Trust requires a high-level understanding of business processes as they are likely to be changed, modified, created, or automated during a digital transformation (sometimes multiple times).

## Note

> This is where **data and asset centricity** *significantly improve organizational agility and productivity*. Think about a business asset as a file with sensitive data that is protected wherever it goes (rather than relying on the security of every system that accesses it). Instead of having to revisit the security of every system that touches this file every time there is a process change, the data file simply stays secure the whole time. Zero Trust limits the effort required to be secure and the friction to adapt processes to new requirements, enabling teams to go faster and be more secure with less effort (and cost). This results in increased business agility and improved productivity of the business, enabling teams to work together toward a common goal instead of working against each other.

- 4. **Develop Zero Trust services and components**: Zero Trust introduces new services and components that will interact with existing technical services and components in the environment. As we develop the Zero Trust solution architecture, we define a set of core Zero Trust services and the components needed to enable them, which are then used across the technology estate to enable business processes.

Some examples are provided as follows:

- **Asset-centric architecture** ensures that all business assets have an identity to enable simple and flexible security decisions. This provides simplicity and clarity that both accelerates business agility and reduces organizational risk. Asset-centric architecture establishes identities, classifications, and/or labels for *all* business assets, including applications, data, users, devices, and so on that persist through their life cycle. This allows easy authoring and maintenance of policies that can keep up with the speed of business (changes in roles, asset sensitivity, business partnerships, vendor relationships, and so on).
- **Adaptive access control** enables organizations to deal with the continuous evolution of relationships with partners and other entities in the digital ecosystem, and the volume of entities involved. It does this by using a policy-based model and supporting it with modern technologies such as AI to dynamically manage changes in an automated manner (including the business model, technology, and security threat/regulatory changes). Finally, it allows the automated self-service and self-registration of people and devices.
- **Data centricity through rights management**: Enables organizations to protect existing sensitive documentation through their full life cycle. This reduces the risk of a data breach (because sensitive documents are always protected) and reduces the cost and effort of doing so (because you don’t have to protect them in dozens of different ways across different systems).
