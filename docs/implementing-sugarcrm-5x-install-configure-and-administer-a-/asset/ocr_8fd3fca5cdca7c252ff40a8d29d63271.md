# # Making changes to SugarCRM modules

Before you begin customizing your system, you should take a moment to identify
the necessary changes and plan them accordingly.

For starters, you should methodically go through all the SugarCRM modules that
you intend to use or are using, and then examine all the terminology (field labels,
tab names, and drop-down values) used within the system by default.

The objective of the exercise is to identify all the areas within the application that
require modification to match your organization's lexicon. Along the way you may
also identify other types of changes that you would like to apply. For example, you
might wish to change the behavior of a default field from a regular text field to a
drop-down field. In other cases, you may find a need to add a new field to track
additional attributes for a record.

Write down the full set of modifications that you identify and keep good track
of them until they have all been addressed. Consult with your peers on the CRM
implementation team to make sure everyone agrees on the necessary changes.

Once you have identified the changes that you need to make, you will want to start
applying them to your system.

# # Non-upgrade safe and upgrade safe

Before we further discuss the process of customizing the system, it is important to
comprehend a critical point regarding the flexibility and extensibility of SugarCRM.

Being an open source application means that all of the code is accessible and
can be adjusted to suit your needs. However, making such changes presents a
problem – upgrades to SugarCRM will not honor those modifications and you
will need to re-apply them manually every time you need to upgrade the system.
These types of modifications are termed as _non-upgrade safe_, and usually involve
modifications to files found in the `include`, `cache`, or `modules` folders of your
installation (among other folders).

You will find that often times, there are multiple ways of applying a specific
modification to SugarCRM. Sometimes the non-upgrade safe approach seems
the more appropriate or simpler solution. However, unless you intend to make
fundamental changes to the inner workings of SugarCRM, it is unlikely that you
actually need to make these types of modifications to obtain your goal. It is generally
advised that you steer away from making non-upgrade safe modifications to avoid
future problems and hassles.