# The intelligent scroll bar

This is one of the features launched in the latest release, and you might have already noticed some colored sections/lines in the scroll bar area. If not, then go back to any of the capture files you have, slowly scroll up and down, and observe the coloring pattern in the scroll bar area. Any guesses what difference it would make in the analysis process? Let's understand this with an example.

I will use a previously captured file for demonstration purpose, which has HTTP and HTTPS packets along with some retransmission and duplicate frames. There is no difference that you can figure out at first glance, but as soon as you start scrolling, the coloring pattern will be shown in the scroll bar area. This pattern is based on the coloring rules that you have in your application. For example, as per the default coloring rules, duplicate and retransmission packets are usually seen with a black background and a red foreground, and HTTP packets are shown with a green background and a black foreground. Now, let's verify this in the application itself. Refer to the following figure for the same:

![](bbox=[0.07, 0.47, 0.93, 0.85])

Figure 9.7: The intelligent scroll bar in action