# Example of CDE dashboard

Now that you know what a Pentaho dashboard is, and the basics around its development, let's see a concrete example defining a complex preview of the `my_first_report` report. If you followed the examples described earlier in the chapter, you should already have a working `my_first_report.prpt` file available in the `/public` path of the Pentaho repository. If not, you can find it in the https://github.com/fcorti/pentaho-8-reporting-for-java-developers repository, specifically, in the `Chapter 15 - Using reports in Pentaho Business Analytics Platform/my_reports` folder.

The example dashboard we are going to build in this section will be composed of a left panel, using most part of the page width, previewing the report using `PRPT Component`, and a right panel, containing two buttons: the Schedule button connected to `Schedule PRPT Component` and the Launch button connected to `Execute Prpt Component`.

Once you have created a new CDE dashboard using the File | New | CDE Dashboard item from the Pentaho user console, in the layout perspective set up the rows and column as shown in the following screenshot: