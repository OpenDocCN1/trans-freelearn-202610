
Establishing the spatial model in COBie is essential to ensure that the locations of all the assets are automatically recorded. The asset types would usually be determined by stage 3, even if they are later changed, and registering what they were before they were changed is valuable information for an asset manager replacing a failed product that may have been "value engineered" with the one the designer originally recommended.

## How can standardized client requirements and standardized product data improve efficiency in the process to deliver asset information?

This is fundamental. The information asset team's need for pumps, fan coils, doors, or cladding are mostly the same across different sectors—residential, education, retail, offices, and healthcare sectors. Clearly, there are differences of criticality even within these sectors, and the client's requirements need to reflect the needs of the different stakeholder groups on each project.

However, by starting from a peer-reviewed set of standard libraries, the amount of effort required to make those changes is minor. By standardizing performance requirements in a machine-readable format, the post-occupancy measurement can inform the effectiveness of a design and, perhaps, modifications to the requirements.

Similarly, manufacturers can create their standardized product libraries to include the information and performance levels clients are asking for and more closely align what they are creating to the client's needs.

The cost of operational effectiveness and environmental performance can be modeled to create far more sustainable buildings.

In simple terms, few buildings, even today, are delivering fit-for-purpose asset information, because it is still such a manual process that is left to the end when time and cost is running out. By standardising the information from the outset, the committed information manager can invest their time on the aspects that really need their skills, leaving the delivery of the asset information to the process.

## What is your view about COBie, now and in future developments? Is COBie a long-term goal or, in the future, could it be replaced by advanced technology to structure data differently?

Everything can be improved, but I am nervous of moving the goalposts when we are finally gaining some traction with people understanding that the data is as (if not more) important as the geometry in BIM.

COBie is a data schema, which can accommodate much more than it is used for at the moment. It isn't perfect but, by using Attributes effectively, we can get it to do 95% of what we need.

It is important to clarify what we mean by the information that asset teams *need*.

They use different applications for different reasons. For example, there are:

- **CMMS**—Computerized maintenance management applications that are principally used to generate work instructions for regular/planned maintenance. This typically would include just 10% of the assets in the O&M.
- **Helpdesk**—These support reactive maintenance, and there is a trend towards using CRM applications for this purpose.
- **H&S/O&M**—Typically, the as-built records of what was installed with the associated documentation. There is a requirement to keep this updated with any changes, but that is seldom done. ActivePLAN delivers this as a Digital O&M or **Asset Information Model (AIM)** that contains everything, including the asset types in the CMMS.
- **Survey and inspection**—These are specialist mobile applications that need very focused information and to be able to deliver that data back to an AIM.
- **Lifecycle management**—This is often done in spreadsheets.
- **Space management**—This is sometimes termed FM and includes room bookings, generally delivered as a separate application (or module) to the CMMS application—for example, IBM Maximo and IBM Tririga.
- **BMS/Digital Twins**—I am connecting these because they are both ways of using/applying the coordinated Asset data that is in the AIM.

A completed COBie data model or Excel workbook should contain all of the information required for each of these applications/uses. However, we should not expect these applications to read a full COBie data model or workbook—or indeed develop an IFC module.

Even the large CMMS applications, who have developed the means to deal with COBie, convert it to import into their database. So, it makes more sense for the handover COBie data to go into an Asset Information Model like ActivePLAN that can hold and manage ALL of the required information and generate tailored outputs for the different CMMS applications plus all of the types of applications mentioned above.

In the modern world of "data services", by ensuring the COBie data schema is complied with, web services can provide the dynamic interoperability that enables the contractor carrying out an inspection/service of fire doors—for example—to record a failure in their CMMS application, raise a "job" to repair it, and feed back to the AIM the changed status of that asset so that an appropriate risk can be assessed in the context of the whole building.

The work we are doing with BRE is teaching us about the use of Ontology and how this can be used to provide a context for determining what needs to be collected and how it should be used. The data scientists we are working with are happy to use COBie as the data exchange mechanism—the key thing is getting the right data for the right purposes.

There have been discussions about removing "Coordinates" from the schema. This would be a real shame because the asset information needs to be "located" and not merely referenced to a space. If you have 20 CCTV cameras in a single space, holding the *x/y/z* coordinates in the data (that is, not a separate geometric model) is essential.

## What are the challenges of integrating operations and maintenance (O&M) and asset management (AM) in the construction industry?

I see Asset Management as the overall process of which operations and maintenance are subprocesses, alongside lifecycle planning/replacement, small works, space management, BMS. They are (and will continue to be) supported by many applications, used for many different reasons by different stakeholders. This is easily addressed if we can overcome the two largest barriers—the quality of data (structure, content, reliability, and currency) and lack of investment.

The latter affects the former.

FM is often the first thing to be cut when money is tight. Money for new developments is often easier to secure than funds to install a new heating or air-conditioning system. Even with PFI, where the importance of investing in facilities is well recognized, contracts are often put in place, where the FM has little choice but to cut corners.

Few projects (even today) are delivering reliable information about the location of fire dampers and fire stopping, so how is the FM supposed to budget and plan for their inspection, maintenance, and replacement?

Clients seldom know what is in their buildings (in some cases, they don't even know how many buildings they have). FMs are often expected to tender for maintenance on existing estates, with very incomplete data about what they will be maintaining. They might commission surveys (they often need to do this on brand-new BIM-delivered buildings). Still, many of the items they are expected to maintain are not easily visible for surveyors to record, so they are not picked up.

## Is a Digital Twin the future of Operations and Maintenance (O&M) and Asset Management integration?

Digital Twin is a term used by different people and with different meanings. I attended the September 2019 Digital Twin event at the ICE in London, where a veteran from Formula One explained that they have been using Digital Twins for 20 years—they used to call it Simulation.

I think that is how we should differentiate between a 3D model, even one with IoT sensors, and a proper Digital Twin that is able to simulate activities and scenarios to inform better asset management.

BMS applications are becoming increasingly sophisticated and are undoubtedly part of this process, as is Artificial Intelligence.

We have been experimenting with AI since 2018 to understand how agents can be trained to respond to a question or an event. My business partner, Tim Aikin, has sensors all over his house, feeding his own Digital Twin, using ActivePLAN AIM as the platform.

We are also experimenting with Graph database, to take the spatial model from a residential tower block, assign the fire protection assets to systems and spaces, and model the impact of a failure of a single asset on the spaces and people within them. Our next step is to create scenarios using AI to simulate how people will respond to an event and how the environment could be improved to make them safer.

The fact is that all of these processes are reliant on better asset information that is kept updated and the more people who are connecting with, and using, that information for different purposes, the better.

## How could Revit and BIM authoring tools improve to facilitate the process to deliver Asset Management? Is data in the Cloud the future of asset management in BIM tools?

All applications can be improved to facilitate the process, but we need to understand that BIM is about the information and not the applications that create it. Holding asset data in an application that is primarily designed to create 3D models is not sensible.

Data is best managed in databases, and the Cloud makes that even easier.

Different stakeholders can be given the rights to use and update elements of the information, ideally from the applications they are using for their day-to-day tasks—not only 3D modeling applications.

Revit and other authoring applications support APIs, so there is no need to add the non-geometric data to the 3D objects themselves. The dynamic connection to a Cloud service gives the impression of the data being in the model when the user clicks on it. Still, it is being managed (and created) elsewhere—often by subject matter experts who have quality-assured the data.
