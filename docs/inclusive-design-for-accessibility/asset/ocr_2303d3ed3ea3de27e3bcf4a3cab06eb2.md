Semantic HTML lays the structural foundation for accessibility by giving content meaning and clarity. But structure alone isn’t enough; how we *style* that content also plays a crucial role in usability and inclusion. Let’s turn our attention to CSS and explore how thoughtful styling choices can support a more accessible experience for all users.

# Styling for inclusive design with CSS

**Cascading Style Sheets (CSS)** is the stylist of modern web design, transforming basic HTML structures into visually engaging and responsive experiences. But the way you implement CSS can make or break accessibility.

Accessibility in CSS isn’t just about looking good – it’s about *working for everyone*. Poorly considered styling can create barriers for users with low vision, cognitive challenges, or motion sensitivities. A high-contrast button could mean the difference between completing a purchase or leaving a website frustrated. A keyboard user relies on visible focus styles to navigate, while someone with motion sensitivity might need animations turned off.

The good news? By adopting thoughtful best practices, you can ensure your CSS-enriched designs are inclusive, usable, and still beautiful.

# Ensure sufficient color contrast

If users can’t read your content, no amount of clever design will save it. **Web Content Accessibility Guidelines (WCAG)** set minimum contrast ratios to ensure legibility:

+   - **Normal text** (such as body copy) must have a contrast ratio of 4.5:1 against its background
+   - **Large text** (18pt regular or 14pt bold) requires a lower minimum ratio of 3:1 because its size makes it easier to read
+   - **Meaningful user interface components** (such as form field borders or focus indicators) and graphical objects (such as icons or charts) also need a contrast ratio of 3:1 with their background to remain clear and distinguishable

Let’s take a look at this example: