# • Objectives

We are not talking about the objectives of the Red Team exercise for the target (Company Z) here. As we discussed earlier, the objective of having a Red Team assessment performed is to test the effectiveness of the Blue Team of an organization. What we are referring to here is the objectives for the Red Team; when they can consider the job completed; the end goal; the finish line. That said, the objective for the Red Team is to infiltrate Company Z's industrial environment and get a new firmware installed on a programmable logic controller (PLC), named Test_Left. No information except for the company name is given, and all tools, methods, and tactics are fair game. Once in the industrial environment, only workstations, servers, and the PLC defined earlier are in scope—anything else is off limits.

The engagement outlines should be well documented and agreed upon and signed by both parties involved. This ensures that there will be no surprises or misunderstandings on either side of the table.

With that outlined, let's start the Red Team engagement by performing some reconnaissance on Company Z.

# Recon

The recon phase of the Red Team engagement is where we collect as much relevant information about the target as we can. At a minimum, we should figure out the physical presence of the target (where they are located), as well as the cyber presence (the public IP address range they own) and the technology they incorporate—for example, are they a Siemens/Rockwell/Emerson shop, do they run a distributed control system (DCS), how do they tie parts of the technology stack together, and so on; and what are the specifics around the technology in use (brand, patch-level/software revisions, upkeep practices, vendors, upstream and downstream partners, and so on). Recon is typically an ongoing process that is built upon and expanded throughout the engagement. You might start by attacking a single location but discover you need to get details on a second location or a supplier or vendor that might allow access when attacking the first location is not fruitful.

# Looking up the company's public IP address range

To keep the example Red Team engagement simple, we are going to assume Company Z operates at a single physical location for which we figured out the address, and did some Google Street View surveillance to assess what we are up against.