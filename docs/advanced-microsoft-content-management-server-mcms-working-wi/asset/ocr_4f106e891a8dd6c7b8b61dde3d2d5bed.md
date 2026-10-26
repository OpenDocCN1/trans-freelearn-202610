# Building CMS Explorer

The *Building Websites with Microsoft Content Management Server* book (Packt Publishing, January 2005, ISBN 1-904811-16-7) makes extensive use of MCMS's Publishing Application Programming Interface (PAPI). We show how to use it to provide custom functionality within template files, to add business processes to workflow events, to tailor the Web Author Console, and to implement forms authentication for the Tropical Green site, which the reader builds as they progress through the book.

The PAPI is in fact a huge library. You could code with it for months and still find new tricks you never knew existed! This is the first of three chapters that compliment the understanding you will have gained from the book and attempt to take your understanding of the PAPI to another level. Follow along as we demonstrate several highly useful techniques and show how they can be leveraged in a real-world scenario, as we apply them to the Tropical Green site.

> **Where can I download a copy of the Tropical Green website?**
> 
> The code files for the Tropical Green website created over the course of the earlier book are available as a download package on this book's download page. Go to the Packt website at `http://www.packtpub.com/support/`, and choose Advanced Microsoft Content Management Server Development in the dropdown.

# A Central Administrative Tool: CMS Explorer

We put some serious thought into creating an example that would not only give you a thorough grounding in the more advanced methods available in the PAPI but would also leave you with a tool that you will find handy in your day-to-day MCMS work. From our own experiences as MCMS developers working in the time-critical world of the software industry, one thing that we have found invaluable has been a custom MCMS administrative tool. In the first three chapters of this book, we walk you through the process of building such a tool, which we will name CMS Explorer.