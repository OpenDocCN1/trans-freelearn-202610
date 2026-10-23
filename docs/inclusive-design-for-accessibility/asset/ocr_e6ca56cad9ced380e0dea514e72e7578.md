Recognize where annotations are and aren’t necessary. If UI designers are embedded directly in cross-functional engineering teams and communicate directly with Android, iOS, and web developers on a daily basis, annotation needs may be minimal. On the other extreme, if the UI design team is siloed and simply tosses artifacts downstream into a waterfall production environment, more verbose annotations will be required.

A minimal annotation library should include options for the following:

+   - Landmarks (header/banner, navigation, main, footer/content info)
+   - Heading levels (h1, h2, h3, etc.)
+   - Alternative text names for images
+   - Control roles (checkbox, radio button, switch, etc.)
+   - Autocomplete attributes (name, address-line1, address-level1, etc.)
+   - ARIA labels (for naming sections, forms, or navigation)

While annotations are useful, they mustn’t be superfluous. For example, in a design artifact where the small screen mobile layout and the large screen desktop layout are side by side, it may only be helpful to annotate the mobile layout, only noting differences or additional features in the desktop layout.

# Alternative text for informative versus decorative images

To achieve a good experience for people using a screen reader, the design system must encourage consuming teams to add alternative text descriptions for informative images and discourage alternative text descriptions for decorative images.

Consuming teams need clear guidance on what constitutes a decorative or informative image and how to describe either. If the design system doesn’t set patterns for alternative text usage, consuming teams will either ignore alternative text, leaving out crucial visual content contained in photos and graphics, or overuse alternative text for decorative icons, leading to repetitive or nonsensical experiences.

By default, UI icons delivered by the design system must be name-agnostic and considered decorative with no alternative text. Design systems typically supply iconic UI imagery that practically never needs a description. Even with a typical set of UI icons, it’s impossible to write a single alternative text description for an image to achieve good UX in all circumstances.