

## CRM Deployment Options

A business with very limited financial resources may conclude that both – Red Hat Enterprise Linux and Windows Server – are beyond their means. Even if you are not in that situation, it is comforting to know that Fedora Core and CentOS are both widely used zero-cost alternatives, and also happen to be derivatives of Red Hat. CentOS in particular is intended to be a clone of Red Hat Enterprise Linux and it has a large following. If you have not heard of it before and are concerned about its viability, the fact that SugarCRM (the company) utilizes 64-bit versions of CentOS for its On-Demand datacenter (at the time of this writing) should help ease your fears.

The final distribution mentioned was Ubuntu. Ubuntu is quite different, in that it has a long history of focusing on growing the adoption of Linux on the desktop. It has gained popularity primarily due to its ease of use. Server installations of Ubuntu are definitely possible and many users today are successfully using it for their SugarCRM implementations. In general, choosing one distribution versus another usually becomes a matter of personal preference and comfort.

The cost involved in purchasing a server operating system has just been explained and the choice is yours. Clearly, were you to base your decision solely on licensing costs, Linux would be a more cost-effective choice. In practice, either of the operating systems is more than capable of fulfilling your CRM needs and in addition, each requires a certain amount of investment to maintain, and these costs are not readily visible up front. As a result, your selection is usually a choice more directly influenced by licensing and ongoing support costs, more so than licensing costs alone.

The level and type of technical resources you have access to will no doubt have some influence on your choice. You will need someone to be on call in case of emergencies, perhaps to come in and set things up for you in the beginning, and possibly to perform backups each week. There are many independent network and server support people who make their living performing this type of work and it should not be difficult to locate a resource. Prices will vary, but working with one of these individuals might cost you US$5,000 to US$10,000 per year and depending on your circumstances, could be a perfect fit.

Of course, the On-Demand option at US$30/ per user/ per month remains an option as well.

## Specifying your server hardware

At home, most of us use our computers as isolated workstations for work or play. A few of us may also connect several computers at home into a network, much in the same manner they are connected at the office. When several computers are connected in a network, there is often value in attaching one or more special computers to the network that is designed to act as a shared resource for all users. These special, typically more powerful computers are called servers.

Most of us are familiar enough with a home or office Personal Computer or PC. The usual product is a so-called three-box configuration – the system unit, the display, and the keyboard. While a computer server can look quite similar to a PC, it has a number of fundamental differences relating to hardware that affect cost.

Note that there are plenty of low-end servers from the likes of HP and Dell that use non-parity memory, **Parallel Advanced Technology Attachment (PATA)** or **Serial Advanced Technology Attachment (SATA)** disk drives, and a single power supply. They can be used for small SugarCRM installations fairly effectively (those not exceeding 10 users). You should, however, always aim for the highest capacity and reliability that you can afford. Remember, it must serve your needs today and in the future, for a minimum of 18 to 24 months. A lower cost solution may seem attractive today, but rarely pays off in the long run. It is usually best to invest a little more in higher quality components up front, rather than pay for problems down the road as they will be more costly.

**Error Correction Codes (ECC)** memory for example, has the ability to detect and correct the most common forms of errors that could be made by semi-conductor memory when it starts to fail. Typical PC memory, by contrast, does not have this capability (due to the lack of a feature called parity) and certainly does not have the ability to correct those errors. The only time a PC checks to see if its memory actually works is at system boot time – when you turn it on. As you intend to leave your server on all the time, that approach clearly will not work.

Similarly, power to a server is typically supplied through two physical power supply units connected to it in a redundant manner. Should one unit fail (not uncommon), the fault is reported, but the system continues to function using the working power supply unit. You would then eliminate immediate impact and be allowed the opportunity to replace the power supply at a scheduled maintenance window of time, as opposed to the time of failure.

A **Redundant Array of Inexpensive Disks (RAID)** hard drive configuration is designed to provide performance and redundancy. These types of configurations require the use of multiple hard drives that work in tandem to split data into smaller chunks that in turn reduce access times. There are varying levels of RAID configurations numbered 0 through 6 (skipping 2). The number of drives required to implement a RAID system will vary depending on the level that you select. It can range from two drives (RAID 0) to as many as four (RAID 6).

## CRM Deployment Options

Redundancy levels are also directly tied to the level that you choose. RAID 0 should never be used in a server environment as it does not offer any sort of redundancy. You should look at using RAID 5 or 6, as they offer the best performance and redundancy. A hard drive failure in such environments would not represent a loss of data. RAID 5 and 6 systems are specifically designed to tolerate such problems without causing loss of data.

The Central Processing Unit (CPU) or processor used in a server is typically a multi-core processor (at least two cores). Depending on your anticipated load, you might be better off with a quad core chip. The general rule of thumb is that the more cores, the better the performance as computing work would be divided amongst the various cores.

System memory size is perhaps the biggest difference between a server and a PC. Most PC users (other than those doing graphics design and other demanding tasks) work happily with 2 to 4 GB of memory in their PC – usually 2 GB is the norm. In contrast, few servers use less than 2 GB of memory, at least those that perform well. Most use a minimum of 4 GB. With more memory, the work being done for many users can stay in memory simultaneously as it is performed, rather than being sent temporarily out to the hard disk if memory runs out of space. As system memory is at least 100 times faster than the hard disk, anything that involves the hard disk will slow down the system substantially. It is also worth noting that database applications, such as SugarCRM, perform better when more memory is made available to the database server.

Some of these differences are summarized in the table that follows:

|    | PC    | Server    |
|---|---|---|
| **Form factor**    | Desktop, Tower    | Tower, Rack mount    |
| **Memory type**    | Non-parity    | ECC (Error Checking and Correcting)    |
| **Memory size**    | 2 GB to 4 GB    | 4 GB to 32 GB    |
| **Hard disk technology**    | PATA / SATA    | SATA / RAID    |
| **Hard disk speed**    | 5,400 RPM to 10,000 RPM    | 10,000 RPM to 15,000 RPM    |
| **LAN interface**    | Ethernet 10/100, Wireless    | One or more Giga-Ethernet    |
| **Power supply**    | Single 250 W to 450W    | Redundant 500W    |
| **Processor (CPU**    | Single Dual Core Chip    | One or more Quad Core Chip    |
| **Video**    | Often High Performance for Gaming    | Low Performance / Generic    |
| **Users**    | One–Local    | Many – Remote    |

