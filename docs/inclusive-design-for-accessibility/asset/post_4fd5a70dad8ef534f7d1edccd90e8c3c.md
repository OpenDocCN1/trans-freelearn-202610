
- Autocomplete helps reduce errors for everyone while speeding up form completion.
- Focusable controls in meaningful sequence lead people using a screen reader on a guided linear journey through the UI.

These considerations lay the groundwork for creating design systems that not only comply with accessibility requirements but also truly empower people who are blind or have very low vision. Now we’ll explore how design systems can further support these users by addressing headings, focus management, alternative text, and assistive technology considerations.

### Choose a non-semantic typographic naming convention

The design system must adopt a naming convention that doesn’t rely on numeric identifiers to indicate size and weight but focuses on the intended usage and style of the text. These simple examples allow for any style to be applied to any heading level. In this manner, h1 can be the smallest type visually, but maintain its meaning to people using a screen reader.

The following styles can be applied to any heading, regardless of its heading level:

| CSS | HTML |
|---|---|
| `.text-normal { font-size: 1rem; }` | `<h1 class=text-normal>My coffee dashboard</h1>` |
| `.text-medium { font-size: 1.5rem; }` | `<h2 class=text-medium>My favorites</h2>` |
| `.text-large { font-size: 2rem; }` | `<h2 class=text-large>Featured roasts</h2>` |
| `.text-huge { font-size: 4rem; }` | `<p class=text-huge animate>Coffee jitters!</p>` |

Figure 10.19: Semantically agnostic styles

Recognize where annotations are and aren’t necessary. If UI designers are embedded directly in cross-functional engineering teams and communicate directly with Android, iOS, and web developers on a daily basis, annotation needs may be minimal. On the other extreme, if the UI design team is siloed and simply tosses artifacts downstream into a waterfall production environment, more verbose annotations will be required.

A minimal annotation library should include options for the following:

- Landmarks (header/banner, navigation, main, footer/content info)
- Heading levels (h1, h2, h3, etc.)
- Alternative text names for images
- Control roles (checkbox, radio button, switch, etc.)
- Autocomplete attributes (name, address-line1, address-level1, etc.)
- ARIA labels (for naming sections, forms, or navigation)

While annotations are useful, they mustn’t be superfluous. For example, in a design artifact where the small screen mobile layout and the large screen desktop layout are side by side, it may only be helpful to annotate the mobile layout, only noting differences or additional features in the desktop layout.

### Alternative text for informative versus decorative images

To achieve a good experience for people using a screen reader, the design system must encourage consuming teams to add alternative text descriptions for informative images and discourage alternative text descriptions for decorative images.

Consuming teams need clear guidance on what constitutes a decorative or informative image and how to describe either. If the design system doesn’t set patterns for alternative text usage, consuming teams will either ignore alternative text, leaving out crucial visual content contained in photos and graphics, or overuse alternative text for decorative icons, leading to repetitive or nonsensical experiences.

By default, UI icons delivered by the design system must be name-agnostic and considered decorative with no alternative text. Design systems typically supply iconic UI imagery that practically never needs a description. Even with a typical set of UI icons, it’s impossible to write a single alternative text description for an image to achieve good UX in all circumstances.

### Intersectionality of disabilities

Out of practicality, accessibility in design is discussed in the context of a single disability or assistive technology, but disabilities are much more complex when people with multiple disabilities are considered.

A design system that accommodates dynamic text sizing and consistent focus styling can accommodate someone with low vision and a severe motor disability. People who find animations or motion distracting may use magnification tools to focus, framing out distracting features. People who have cognitive disabilities may use a screen reader or closed captions to help with reading comprehension or understanding audio content. Real-world scenarios are rarely so neatly separated. Involving users with intersecting needs – such as someone who uses a screen reader and also experiences motion sensitivity – helps teams understand and design for complex, overlapping challenges.

When design systems are built with flexibility, clarity, and alternative ways to interact, they can better accommodate this complexity without requiring exceptions or additional customization. It’s important to remember that many people experience multiple disabilities at once, such as low vision and motor impairments, or hearing loss combined with cognitive challenges. A design system that anticipates these overlapping needs – such as ensuring high-contrast typography, consistent focus styles, clear labels, and single-point activation – creates a more equitable and consistent experience for everyone.

### Features to omit for everyone with disabilities

Accessibility for design systems doesn’t just mean making everything accessible; it means deciding what components and patterns to omit. Some common UI components can be left out of a design system to encourage consuming teams to seek better solutions. Collaborating with users to understand why certain components create barriers is essential – this helps design systems evolve to support inclusive experiences rather than simply removing features.

The native HTML `select` element can be completely styled in its collapsed state while the options surface styling is provided by the operating system and does not require redesign, though future CSS proposals may allow for more styling options.

![](img/3bdc9e2d41de898f8a3bcc9634b0000e_266_0.png)

*Figure 10.23: The native HTML select surface in macOS*

### Carousels

Carousel slideshows are often a compromise between stakeholders who couldn’t decide what content should lead, so they choose all of it. This results in hostile user experiences that rarely receive engagement beyond the first slide.

Future browser features may include native-like support for accessible carousel patterns, but this doesn’t mean it’s the best way to convey content. Even accessible carousels deliver an additional cognitive load for all people who must learn how to navigate its special compound UI and consume its unique content. Is it swipeable? Do the arrow keys operate it? How many slides are present? This and more information must be conveyed and understood by the user, and people must be appropriately motivated to engage beyond the initial view.

If carousels are an unavoidable component of the design system, help teams use accessible carousels wisely to eliminate barriers, reduce cognitive load, and add value to the experience:

- Avoid using auto-advance. This creates barriers for people with cognitive disabilities and vestibular disorders. If featured, auto-advance must be off by default and user-controllable.
- Guide teams to favor predictable information. Carousels work better when people can guess what’s coming (e.g., shoe colors and product variations). Avoid surprise or disjointed content across slides.

## 11 Tools and Techniques for Accessibility Evaluation and Prioritization

By Kai Wong, CPACC, CHES®

Accessibility is about creating digital experiences that work for everyone. Even if a design looks accessible, many accessibility issues live under the hood – within the code.

Accessibility testing bridges the gap between design intent and actual user experience. It is an important part of quality assurance (QA). Just like with any other quality check, evaluating product accessibility involves testing it.

> The Web Content Accessibility Guidelines (WCAG) is a widely accepted technical standard that can guide digital accessibility testing. You can find the latest version of the guidelines on the World Wide Web Consortium (W3C) website (www.w3.org).

This chapter provides an overview of digital accessibility testing tools and techniques, focusing on web and mobile app accessibility testing, its challenges, and common issues to look out for. You’ll learn how to combine automated and manual testing methods, prioritize what to fix, and bring accessibility into development workflows. The goal? To help you and your team test smarter and build more inclusive products.
