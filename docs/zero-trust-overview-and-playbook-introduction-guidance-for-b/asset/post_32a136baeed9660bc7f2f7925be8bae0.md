
- **Data-centricity support through tokenization services**: These reduce the number of copies of sensitive data that the organization has to protect. Setting up a tokenization service allows data to be shared with reduced risk by replacing multiple copies of sensitive data with a reference to a single well-protected main copy of the sensitive data. This also empowers business, technology, and security teams dealing with agile delivery through dependency reduction (teams don't have to wait on other teams to update their encryption component or firewall, and so on) and becomes a huge enabler in frictionless evolution. As teams deal with different scenarios—**software as a service (SaaS)** providers, new partners and clients, entry into new business domains, or scenarios such as remote work and evolving organizational structures—this becomes a source of competitive advantage, reducing cost and risk and increasing agility.
- **Endpoint and application threat detection**: Organizations often develop components and services that enable security analysts to detect and clean up infections on computer operating systems (aka endpoints) and applications. As more and more teams develop websites and mobile applications to enable digital experiences, these development teams should use common Zero Trust components so that they don't have to build their own component (or operate without this basic security protection).
- **Privacy-by-design programs**: Organizations frequently develop a program to manage privacy in response to the constant flow of privacy regulations, such as the European Union's **General Data Protection Regulation (GDPR)**, the **California Consumer Privacy Act (CCPA)**, or Brazil's **Lei Geral de Proteção de Dados (LGPD)**.

Each of these operational pillar elements is described in much more detail later in the playbook.

## Acme example – operational pillar

Acme now transitions to operationalizing its roadmap and focusing on making it *actionable* to capture quick wins and make continuous incremental progress. This requires understanding the operational model of the business and mapping Zero Trust to it. *Figure 8.10* illustrates the operational pillar at *Acme Bank*:

![](img/c2bdb6ffe8b251139bc9db4439c8ed5a_166_0.png)

### Figure 8.10 – Acme Bank’s operational pillar

**For its product model**, Acme's products (goods and services) are retail and government banking.

Currently, Acme's **business functions** to support this product model are organized under retail banking and government banking, but Acme found this duplication of each capability slows down agility. As a part of its roadmap, Acme is reorganizing all departments against its business capabilities—deposits, risk management, payments, mortgage banking, cards, and unsecured loans—with each supporting both retail and government markets as appropriate via sub-capabilities.

Acme's Zero Trust initiative is the security part of this roadmap and focuses on the early prioritization of the following:

- **Asset-centric architecture** to simplify Acme's ability to apply policy across many individual assets, setting up a foundation for adaptive access control
- **Adaptive access control** capability to ensure strong access control for assets during regular and expected process changes
- **Data centricity** to simplify sharing data securely and reduce friction during the frequent evolution of business processes
- A centralized **privacy-by-design** capability to allow easy entry and access to new jurisdictions

## Zero Trust integration changes Acme’s business processes:

- **Asset-centric architecture** creates a responsibility for *Acme* business leaders to identify business owners for its most valuable assets to determine risk and access policy. This ensures that the risk to these assets is understood and managed well by those who understand the impact of these business assets, rather than relying on security teams to protect all assets equally without context.

*Acme* expects this will reduce the “blast radius” of damage and business impact for common attacks such as ransomware or **denial of service (DoS)**. This is because *Acme*’s policy-based access will reduce the potential number of assets that can unnecessarily communicate with each other and be affected (similar to how reducing the amount of human contact limits the spread of a disease). Coupled with adaptive access control, this will also allow *Acme* flexibility to deal with an evolving digital ecosystem.

- **Data centricity** speeds up *Acme*’s partner onboarding for credit/debit card authentication because *Acme* no longer needs to share full copies of sensitive data to onboard a partner. It also significantly reduces the risk profile of the organization and makes compliance simple to implement, as the number of processes with sensitive data is reduced.

- **Adaptive access control**: *Acme* will build on the asset-centric architecture by implementing modern policy-based access control to provide flexibility and strong security that doesn’t interrupt users and disrupt business processes.

The policy-based enforcement provides the agility to keep up with the fluid evolution of business relationships—today’s partner in a digital world might be tomorrow’s subsidiary and then a competitor or client shortly after that.

Adaptive access control also integrates silent security mechanisms, such as TI and **behavior analytics (BA)** to catch anomalies such as known bad locations, deviations from normal user behavior patterns, and connections using noncompliant or infected devices. Additionally, *Acme* is adopting strong authentication that provides strong (often **passwordless**) logins, which are both easier to use and more secure.

*Acme* focuses on *reusable* **Zero Trust services and components** that enable these capabilities for multiple business processes and functions. *Acme* is prioritizing the creation of an adaptive access control architecture, a data classification scheme, a token architecture and token service, a privacy-by-design service, and an asset-centric architecture.

When procuring and building these components and capabilities, *Acme Bank* will align these with different business functions so that changes in business processes and organizational structural impact are seamless. *Acme* will also map its current technology estate and security policies against industry standards such as the **SOA Reference Architecture (SOA RA)** standard and the Zero Trust Reference Model. This allows *Acme* to use existing infrastructure and industry standards to inform them on what to procure (or build), how it fits into current technology estates, and how it impacts the business.

This approach ensures that Acme’s Zero Trust initiatives are actionable and produce the desired and promised outcomes, ensuring the roadmap enables and secures Acme Bank in a world of continuous change.

## A mindset shift at Acme Bank

Acme recognizes that this is a significant shift in mindset.

Acme’s current model is analogous to protecting a country’s leader and their data by protecting their house (for example, the network) and restricting them to never leaving it.

Acme is focusing on shifting its mindset to the Zero Trust approach of attaching security to the person and their data wherever they are and go. This gives Acme the freedom to do business anywhere, securely.

## The operating model pillar

> > "Bad companies are destroyed by crisis, good companies survive them, great companies are improved by them."
> > 
> > — Andy Grove

You always have to steer the car as you go, no matter how well you have planned your journey. The operating model elements monitor and adjust the strategy and the execution as you go, keeping them aligned and on track, as shown in Figure 8.11:

![](img/c2bdb6ffe8b251139bc9db4439c8ed5a_168_0.png)

Figure 8.11 – The operating model pillar

The operating model pillar reflects and governs the way that the organization operates. Zero Trust is an enterprise-level initiative that needs to be integrated into and harmonized with the organization’s business operating model to be successful.

This starts with awareness—is the organization operating as a single monolithic organization? Or multiple independent subsidiaries? Or something in between?

A major change such as this also requires recognizing that it impacts more than just the literal business and technology capabilities; people change too. People make decisions and do the work, the processes they follow will keep the business going, and the organizational culture will guide decisions when people face a new or difficult situation.
