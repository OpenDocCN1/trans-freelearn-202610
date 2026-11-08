# Paging

Certain output formats, such as HTML, CSV, RTF, and Excel Output, treat the report (no matter how long the report is) as a single page. Use the page header and footer with the knowledge that many output types will not render identically.

In relation to paging, Excel output offers the ability to treat sheets as pages, as long as explicit page breaks are specified in the report bands using the `page-break-before` and `page-break-after` style attributes.