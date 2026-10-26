# 10

# Staging Static Pages

Websites today are dynamic and versatile. Dynamic sites offer many advantages over static sites. Using MCMS, you get to personalize and work with the PAPI to create a truly interactive and personalized user experience. For example, we have seen how navigation structures can be hidden or shown based on an individual's access rights. In fact, content gets updated so frequently and the demand for interactivity is so high that content management systems, like MCMS, assemble pages on the fly. At the very most, snapshots of pages are cached in memory or on the server's hard disk to speed up the process of delivering pages. The cache is invalidated after a certain time period has elapsed or when the content changes. Nothing is ever frozen: only fresh content is served.

In a book about creating dynamic sites, you may find it strange that this entire chapter is devoted to staging static pages. The truth is, despite the fast-paced demands of technology, there is a need to slow things down a little. Static copies of an entire website can be useful on occasions such as:

+   - Creating pages to be packaged on offline media like CD ROMs. User guides and help files are often included with shrink-wrapped software in the form of electronic documents on CDs. Even Microsoft help files (or CHM files) are themselves built from static HTML files.
+   - Taking a snapshot of the site for archival or back-up purposes.
+   - Creating static files for indexing by Index Server or other search engines that require the presence of physical files.
+   - Sharing content across multiple servers. Yes, you could install MCMS on the other servers, or even syndicate content using web services. However, sometimes, the best solution is the simplest—just make static copies of shared content and use a good file copy technique like FTP to transport them (and sidestep the need to acquire more production MCMS licenses).

If you need to generate static versions of MCMS websites, there are two options available:

+   - Use the Site Stager utility that ships with MCMS 2002.
+   - Write a custom staging tool that generates a static snapshot of each page.

> Important Note: Site Stager only stages static pages from postings based on ASP templates. It does not work for postings based on ASP.NET templates.