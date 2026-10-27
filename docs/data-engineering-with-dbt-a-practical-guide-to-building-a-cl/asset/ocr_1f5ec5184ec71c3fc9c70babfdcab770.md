# S5 – development and verification of the report in the BI application

This is the final story, where you will use the data mart verified in S4 to build the report using the BI application.

The starting point is the data model for the data mart designed in the S1 story, which was developed with the BI tool needs in mind and will, therefore, provide the correct model for your BI tool, whether it is a star schema or a wide table.

We will not go into further details for this story, as this work is done outside of dbt and your data platform, depending very much on the BI tool at hand.

> **Agility tip**
> 
> We have just discussed the usual steps, codified in stories, that are needed to build a report backed by a data platform.
> 
> The process applied is general (know what you want, find out how to build it, build it, verify the data, and deliver the visuals to your audience), but it is important to remember that in an agile team, you do not interpret these steps in a waterfall-like mindset, completing (and signing off) one step before moving to the next. The right approach is to have them overlap and clarify more details as you get closer to the implementation, circling back and rethinking if the data says a different story to what you were thinking.
> 
> Keep the goal in mind – delivering to the customer, not building documentation.

In this section, we have discussed how to work in an agile way and taken a deep dive into building star schema data marts to power your reports, as this is one of the most common uses of dbt and data warehousing in general.

# Summary

In this chapter, you learned some agile philosophy and practical hints, starting from the commencement of your project, managing the backlog, and finally, taking a deep dive into how to organize your work to build reporting data marts. Good job!

In the next chapter, you will learn about collaboration and using Git alongside dbt Cloud to collaborate.