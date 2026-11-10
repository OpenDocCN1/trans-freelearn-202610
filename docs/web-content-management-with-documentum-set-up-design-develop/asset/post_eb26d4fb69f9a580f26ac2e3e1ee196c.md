
## 4 Web Content Management System

Having discussed some of the key Documentum features in the preceding chapters, let us commence our journey into the world of Documentum by helping an actual enterprise deal with the age-old problem of managing large volumes of content in the website.

It is a safe assumption that whoever is reading this book is either genuinely interested in knowing more about Documentum or perhaps needs to evaluate if it could be a good content management option to meet his or her specific business requirements.

Either way, we should first find out if there really is a need for web content management (WCM in short) in the project at hand. All WCM systems have their own share of merits and demerits. We should meticulously analyze the business requirements to verify that the business needs can be fulfilled by a good content management system.

Documentum is an enterprise content management system and offers a multitude of rich tools to cater to the needs of ever-growing businesses. However, let's not forget that there is a caveat. Good CMS systems are costly and entail training overheads for the end users who eventually use them. Additionally, we need to have a development/system team to set up and configure the CMS system and a system administrator to monitor the CMS system as well to ensure smooth operation.

### 4.1 When Should you Use a WCM System?

Documentum as a WCM system is fruitful when the business requires frequent updates to the content on the website/portal. What this means is, a business that needs to keep on updating its business information displayed on its websites to ensure that the website users can view the most up-to-date information available at a given point of time is a good candidate for a WCM.

This can be better explained through a few examples:

- **An online shopping website:** An online shopping website uploads its product information such as product descriptions, product images, price, and stock availability details etc. At any given point of time, this information needs to be up to date, lest the customer purchases something at a stale cost rate. The stock information, if not updated on the site, would result in purchase orders for a product that is not available in the shop!

Instead of updating the website's HTML/JSP pages manually on a daily/weekly basis, the online shopping website firm can manage it effectively through a WCM system. Latest content information can be updated through the WCM content authoring tool and new product information can be easily entered without requiring any knowledge of web scripting tools.

- **An online newspaper website**: A newspaper website's structure/layout remains more or less constant but its actual content changes everyday. In fact the old news expires everyday from the site and is archived in some cases as well. Moreover, the news articles surfacing on the site have to pass through a group of reviewers via a workflow mechanism before they get finally approved. With a WCM system installed, content creators and editors need not learn web technologies/scripting languages in order to upload/edit content on the site. Once trained, they can author content via the authoring tool(s) provided by the content management system itself. So, a newspaper site can be considered a good candidate for a WCM system.

### 4.2 When Should you Not Use a WCM System?

This is a very debatable area and can vary as per the type and nature of one's business and customer needs.

However, the following may be considered as some handy tips based on past experiences with projects involving 'content-heavy' websites. It should be noted that these are personal view points and should not be deemed to be universal industry-standard or globally accepted rules.

Stop and think twice before starting off with a WCM system if:

- Most of the data on the website is fairly static and less prone to updates.
If you do not require making frequent content updates on the site, try and come up with an alternative mechanism for managing the content. Using an automated content management system in such scenarios might turn out to be overkill and result in extra and unwanted costs.

- You have neither the time nor the money for training the employees.
For smaller firms running on low revenue margins, purchasing a CMS system, getting it installed/set-up by a bunch of software developers/experts, and later on getting the business users trained on using the system can turn out too costly an affair. Such organizations should analyze and plan their expenses and/or time and work out the resource requirements in detail before taking the plunge.

- There isn't much content to be displayed on the site.
There are a few transaction-heavy sites such as banking sites, online flight booking websites, etc. that do not require a lot of data to be fed manually by the site content contributors.

Only websites that require managing a wide array of unstructured data should be considered right candidates for a content management system.

### 4.3 Documentum WCM Architecture

If you have done your homework well and are convinced of your business requirement for an effective Web Content Management system like Documentum, then welcome aboard!

Let us discuss a typical Web Content Management architecture using the Documentum product suite.

What we shall discuss going forward is not the 'best possible' WCM architecture, but a reasonably standard one that meets most of the common web-based content needs for many enterprises.

An advanced WCM architecture is not just limited to the products/components depicted in figure 4.1. Depending on one's business requirements, there could be variations in the design and architecture and some more components could be added or existing ones removed from the figure 4.1.

Do not worry if you are unable to understand all the components depicted in the architecture. As you continue reading the rest of the book, all of these will get clearer and more comprehensible.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_60_0.png)

Figure 4.1: WCM architecture using Documentum

### 4.4 How do you Enter Content in the System?

**Web Publisher** is one of the simplest tools to enter content in the Documentum system and forms the entry point into a WCM system. It is a web-based client tool that connects to the Content Server Docbase and assists content authors, managers, and administrators in creating, managing, and administering content for a business site.

### 4.4.1 Web Publisher

The good part about using Web Publisher is that it simply requires a one-time setup of the Web Publisher system and the business-specific customized files on an application server. The Web Publisher end users (or clients) need not perform any installation on their laptops/desktops and can simply access the Web Publisher URL via a range of supported browsers.

The dmc1.ini file on the application server host where Web Publisher has been installed points to the correct DocBroker and port. It is through this that the connection to the Docbase is made.

Sample dmc1.ini file:

```
[DOCBROKER_PRIMARY]
host ==<<Your DocBroker host Name>>
port =1489 # default port
```

The wide range of operations available in Web Publisher is not just limited to creating or managing content for the website. Web Publisher is an integral part of the Documentum WCM architecture and has an intuitive GUI that allows:

- Setting up groups, users, and roles for the end users of Web Publisher
- Creation of intuitive categories or taxonomies, using which business users can create classified content for their organization.
- Creation of delivery (web) cabinets from where the created content can be published out of the Documentum system.
- Setting up security for Docbase objects by attaching Permission sets (ACLs) to the objects in question.
- Promoting and demoting objects in Lifecycle states. Using Web Publisher's 'Promote', 'Demote' and 'Power promote' features, one can push a document to the next state(s) in its lifecycle or demote it to its previous state(s).
- Creation of content templates and associated rules and presentation files, so that users can simply use the templates to create content without learning any web scripting technologies.
- Getting the content items reviewed and approved by other users by pushing them to workflows. It is worth mentioning that Documentum Web Publisher provides some out-of-the-box workflows available for use. Examples: Request new content, submit to website workflow, etc.
- Integration with utility services and tools like automatic property extraction, Content Intelligence services, Inter-Enterprise workflow services, In-Context editing, eWebEditPro Rich Text Editor, etc.
