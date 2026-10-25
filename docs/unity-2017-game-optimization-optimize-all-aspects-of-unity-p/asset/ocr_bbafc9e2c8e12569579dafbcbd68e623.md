# # The Frame Debugger

Before we dive into how batching can save us Draw Calls, let's explore a useful tool, which
can help us determine how batching is affecting our Scene--the Frame Debugger.

We can open the Frame Debugger by selecting **Window | Frame Debugger** from the main
window or clicking on the **Frame Debugger** button in the **Breakdown View Options** in the
Rendering Area of the Profiler. Either approach will open the **Frame Debug** window.

Clicking on the **Enable** button in the **Frame Debug** window will allow us to observe how
our Scene is being constructed, one Draw Call at a time. The following screenshot shows the
user interface of the Frame Debugger, with a list of GPU instructions in the left-hand panel,
and more detailed information in the right-hand panel:

![](bbox=[0.1055, 0.3609, 0.8945, 0.8109])