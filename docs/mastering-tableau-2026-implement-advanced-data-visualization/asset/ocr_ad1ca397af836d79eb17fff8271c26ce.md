> What About Performance?
> 
> You might wonder if all this linking slows things down. Tableau's engineers thought of that. The semantic layer is optimized to push calculations down to the underlying database whenever possible, ensuring you don't sacrifice performance for governance. In many cases, queries run just as fast as they would against the original source.

# Display Data Model for visual understanding

Have you ever opened a published data source created by a colleague and thought, "How do these tables relate?" Maybe you've spent frustrating minutes (or hours) trying to figure out whether to join or blend, or which fields connect which tables. If so, the Display Data Model feature is about to become your new best friend.

When you open a published data source in Tableau Cloud or Tableau Server, look at the Data Source pane. You'll now see a visual representation of the data model. It is complete with tables, fields, and the relationships between them. It's the same kind of visual you see when building a data source yourself, now available for anyone using that published source.

Understanding the shape of your data is fundamental to accurate analysis. If you don't know how tables relate, you might create calculations that return the wrong results without realizing it. The visual model eliminates guesswork. You can see at a glance:

+   - Which tables are in the data source?
+   - How they connect (relationships, joins, or blends)
+   - Which fields are used in those connections
+   - Whether tables use live connections or extracts

This complements the lineage view we discussed earlier. Lineage shows you where data comes from and where it flows. The display data model shows you how it's structured right now. Together, they give you complete visibility into your data assets.

# Recycle Bin

You don't know me as a storyteller necessarily, but the recycle bin deserves story time. A few years ago, a friend of mine was preparing a major presentation for her company's executive team. She'd spent three weeks building a complex dashboard with multiple sheets, calculated fields, and carefully crafted tooltips. The day before the presentation, she was cleaning up her Tableau Server site and accidentally deleted the wrong workbook. Poof. Three weeks of work, gone. She spent the next 12 hours rebuilding from memory and barely slept before her presentation.