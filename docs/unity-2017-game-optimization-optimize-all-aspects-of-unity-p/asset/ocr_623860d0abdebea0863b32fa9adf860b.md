# # Use empty UIText elements for full-screen interaction

A common implementation in most UIs is to activate a large, transparent interactable element that covers the entire screen, forcing the player to handle a popup before proceeding, while still allowing the player to see what's going on behind it (as a means of not ripping the player out of the game experience entirely). This is often done with a `UIImage` element, but unfortunately this can break batching operations and transparency can be a problem on mobile devices.

A hacky way around this problem is to use a `UIText` element with no **Font** or **Text** defined. This creates an element that doesn't need to generate any renderable information and only handles bounding-box checks for interaction.

# # Check the Unity UI source code

Unity provides the code for its UI system in a bitbucket repository found at `https:// bitbucket.org/Unity-Technologies/ui`.

If we're having significant problems with the performance of our UI, its possible to look into the source code to figure out exactly what might be going on and hopefully discover ways to get around the problem.

A more drastic measure, but a potential option, could be to actually modify the UI code, compile it, and add it to our project manually.

# # Check the documentation

The tips mentioned previously are some of the more obscure, undocumented, or critical performance optimization tips for the UI System. There are a number of great resources on the Unity website that explain how the UI System works and how best to optimize it, which is far too large to fit in this book verbatim.

Start with the following page and work your way through them for many more helpful UI optimization tips: `https://unity3d.com/learn/tutorials/temas/best-practices/guide- optimizing-unity-ui`.