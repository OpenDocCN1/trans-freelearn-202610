We will use “issues” and “bugs” interchangeably throughout this chapter. Also, the following information applies to both the web and mobile, unless specifically mentioned otherwise.

While I won’t go deep into WCAG in this chapter to keep things approachable, most of the testing strategies discussed are related to WCAG standards. For specific criteria or to dive deeper into each testing area, refer to the official WCAG documentation.

# Keyboard navigation is key

Some people with vision or mobility disabilities may use alternative input methods, such as a keyboard, to navigate digital content. Websites and mobile apps should be fully usable without touching a screen or clicking a mouse. As a result, the focus order – the order of elements that receive focus when navigating with an alternative input method – and keyboard navigation are essential to test.

If you have a mouse, just let your cat play with it while you complete your manual tests. Meanwhile, you can test the focus order and make sure that all elements can be reached using the keyboard alone.

You can test keyboard navigation on mobile devices using a wireless keyboard, such as a Bluetooth keyboard.

Starting at the top of the web page, use the Tab key to move from interactive element to interactive element. Examples of interactive elements include buttons, links, drop-down menus, sliders, toggles, checkboxes, and text fields. Tabbing should move you through the page and show a visible focus indicator (i.e., outline) on the focused element.

Make sure the focus order is logical (which typically means that moving forward with your keyboard matches the order and meaning of the page) and fairly predictable. It should not jump around on the page like burning garlic on a hot stove. In most cases, the focus should move from top to bottom and left to right. If you’re tabbing and suddenly your focus does a disappearing act… congratulations, you found a bug!