# # 1 Installation

In this chapter we walk you through the installation and configuration of MCMS 2002 Service Pack 2 (SP2), along with SQL Server 2005 and Visual Studio 2005 on a single developer workstation. In addition, we will cover the changes to the SP2 development environment and a number of tips for working within it.

This chapter assumes you are already familiar with the steps necessary to install MCMS 2002 SP1a as detailed in depth in the previous book, Building Websites with Microsoft Content Management Server from Packt Publishing, January 2005 (ISBN 1-904811-16-7).

There are two approaches to setting up a development environment for SP2: upgrading from a previous SP1a installation, or starting from scratch and building a fresh installation including SP2. We will cover both approaches in this chapter.

For the examples in this book, we will be using Windows XP Professional SP2 as our development workstation. However, where there are significant differences for a Windows Server 2003 SP1 machine, those will be noted. All examples assume the logged-on user is a local machine administrator.

# ## Overview of MCMS 2002 Service Pack 2

As with other Microsoft Service Packs, one major purpose of SP2 is to provide an integrated installation for a large number of previously released hotfixes. SP2 will now be a prerequisite for any future hotfix releases.

While many customers will view SP2 as a regular Service Pack, it also offers support for the latest development platform and tools from Microsoft, namely SQL Server 2005, .NET Framework 2.0 and ASP.NET 2.0, and Visual Studio 2005:

+   - **SQL Server 2005**: MCMS databases can be hosted by SQL Server 2005, offering numerous advantages in security, deployment, and most significantly, performance.