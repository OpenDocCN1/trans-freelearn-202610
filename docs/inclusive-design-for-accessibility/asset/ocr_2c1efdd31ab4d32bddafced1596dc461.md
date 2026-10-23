By prioritizing plain language, sensible input design, and minimal distractions, design systems can foster a more inclusive experience that empowers people with cognitive disabilities to participate fully.

# Motor disabilities: designing for imprecise actions

Design can accommodate people with motor disabilities who simply have difficulty precisely interacting with the UI but aren’t using other assistive technology.

It’s not difficult to understand that bigger buttons are easier to click on for everyone. People with difficulty using a mouse or tapping their screen with precision will encounter barriers when buttons are too small or packed too closely together.

To help, encourage mobile-first design with generously sized controls. While it’s common for consuming teams to feel pressured to make densely featured interfaces, the design system examples and documentation must provide examples supporting an accessible target size.

To meet minimal WCAG AA target size requirements, objects must be 24x24 px in size with a 4 px margin. A more generous size of 44x44 px meets WCAG AAA and HIG requirements, and Android Material Design calls for a 48x48 dp minimum size.

![](bbox=[0.21, 0.5, 0.79, 0.68])

Figure 10.13: WCAG AA minimum target size and AAA target size

For HTML-based controls such as checkboxes, radio buttons, expanders, and switches, the entire label must also be clickable, not just the control itself.

![](bbox=[0.25, 0.78, 0.75, 0.86])

Figure 10.14: Web-based controls such as switches must activate when clicking on the label and the control itself