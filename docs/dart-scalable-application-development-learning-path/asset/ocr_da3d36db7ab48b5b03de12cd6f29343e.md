# Live Data Collection

> "It is a capital mistake to theorize before one has data."
> 
> — Sherlock Holmes

Most people in the IT world are familiar with the phrase **Garbage In, Garbage Out (GIGO)**. If bad data goes into a system, the output will be wrong too. A source of good data should always be used, and systems need to be robust enough to handle users or other systems' "garbage". Specifications and standards can help, but there will always be exceptions and gray areas.

One of the most baffling statements I have heard in my career was from a customer with a software data input issue: 'we have our own ISO standard'!

# Kicking off the earthquake monitoring system

The next series of chapters in the book will be built around a single project from several different perspectives. A real-world data source with earthquake information about the planet we live on will be tracked, recorded and displayed.

It is quite ambitious and there will be a lot of Dart along the way. It is surprising how much activity there is in the Earth's crust—perhaps you could blog or write a presentation on seismology using the output of our projects so far!

The project for this section is called `QuakeMonitorFS`—make sure that you select the project with the name ending FS and not DB. The FS version saves to the filesystem while the DB version saves to the database. We will first look at the filesystem storage version, and then move on to the database version.