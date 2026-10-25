# # Save

The **Save** button saves any Profiler data currently presented in the Timeline View to a file.
Only 300 frames of data can be saved in this fashion at a time, and a new file must be
manually created for any more data. This is typically sufficient for most situations, since
when a performance spike occurs we then have about five to ten seconds to pause the
application and save the data for future analysis (such as attaching it to a bug report) before
it gets pushed off the left side of the Timeline View. Any saved Profiler data can be loaded
into the Profiler for future examination using the **Load** option.

# # Frame Selection

The **Frame Counter** shows how many frames have been profiled and which frame is
currently selected in the Timeline View. There are two buttons to move the currently
selected frame forward or backward by one frame and a third button (the **Current** button)
that resets the selected frame to the most recent frame and keeps that position. This will
cause the Breakdown View to always show the profiling data for the current frame during
runtime profiling and will display the word **Current**.

# # Timeline View

The Timeline View reveals profiling data that has been collected during runtime, organized
into a series of Areas. Each Area focuses on profiling data for a different subsystem of the
Unity Engine and each is split into two sections: a graphical representation of profiling data
on the right, and a series of checkboxes to enable/disable different activities/data types on
the left. These colored boxes can be toggled, which changes the visibility of the
corresponding data types within the graphical section of the Timeline View.

When an Area is selected in the Timeline View, more information for that
subsystem will be revealed in the Breakdown View (beneath the Timeline View) for the
currently selected frame. The kinds of information displayed in the Breakdown View varies
depending on which Area is currently selected in the Timeline View.

Areas can be removed from the Timeline View by clicking on the X at the top-right corner of
an Area. Recall that Areas can be restored to the Timeline View through the **Add Profiler**
option in the Controls bar.