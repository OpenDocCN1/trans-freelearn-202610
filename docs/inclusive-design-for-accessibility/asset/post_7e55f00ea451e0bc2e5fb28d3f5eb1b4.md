
The first rule is critical: use native HTML elements first. Before you reach for ARIA, take a close look at what semantic HTML can already do. It’s the quiet overachiever of web accessibility – built-in elements come with meaningful roles, behaviors, and keyboard support that sometimes even ARIA can’t replicate.

If a native element exists, use it. ARIA is a powerful tool, but it should be your last resort – not your first instinct. Nothing says “I didn’t do my research” like slapping role="button" on a `<div>` when you could’ve just used `<button>`.

Before applying ARIA customizations, always refer to the latest specifications to ensure you are using roles, states, and properties correctly. Guessing or misapplying ARIA can lead to confusion for assistive technologies, ultimately harming rather than helping accessibility. For example, adding role="navigation" to `<button>` instead of using a proper `<nav>` element may confuse screen reader users by misrepresenting the element’s function and purpose.

The latest ARIA specification, WAI-ARIA 1.2, became a W3C Recommendation on June 6, 2023. It provides detailed guidance for using roles, states, and properties effectively: https://www.w3.org/TR/wai-aria-1.2/. For further best practices, consult the Using ARIA guidelines, published as a W3C Working Draft on September 27, 2018: https://www.w3.org/TR/using-aria/.

Now let’s take a closer look at the two ARIA features we mentioned at the start of the section: aria-expanded and aria-labelledby.

## aria-expanded

The aria-expanded attribute communicates whether content associated with an element is currently displayed (true) or hidden (false). Dropdowns, accordions, and disclosure panels are common use cases for this attribute. Non-sighted screen reader users especially benefit from its presence, as they cannot rely on visual cues such as arrow icons or sliding animations to understand whether content has been revealed.

Consider a common FAQ interaction, where a user clicks a question to reveal the answer. Visually, the change is obvious. But for screen reader users, we need to communicate whether the associated content is expanded or collapsed. The aria-expanded attribute makes that possible.

```
HTML
<button aria-expanded="false" class="faq-toggle">
    What is your return policy?</button>
<div class="faq-answer hidden">
<p>You can return items within 30 days of purchase for a full refund.</p>
</div>

CSS
.hidden {
    display: none;
}

JavaScript
const button = document.querySelector('.faq-toggle');
const answer = button.nextElementSibling;

button.addEventListener('click', () => {
    const expanded = button.getAttribute('aria-expanded') === 'true';
    button.setAttribute('aria-expanded', String(!expanded));
    answer.classList.toggle('hidden');
});
```

This setup uses aria-expanded to communicate the state of the answer content, giving screen reader users the same context that sighted users get from visual cues. The `.hidden` class ensures the content is visually hidden without relying on the HTML hidden attribute, which behaves differently and can cause issues with toggling in JavaScript.

## aria-labelledby

aria-labelledby is used to programmatically associate a visible text label with another element on the page. This is especially useful when the text label is not nested inside the element being labeled but is visually associated with it. A common example is labeling a form section using a heading that describes its purpose.

Imagine a checkout page where the payment form is visually introduced by a heading. To ensure screen readers announce the form with a meaningful name, the developer can link the form container to the heading using aria-labelledby:

```
<h2 id="payment-heading">Payment Information</h2>
<form aria-labelledby="payment-heading">
    <!-- Form fields here -->
</form>
```

These are just two examples of the many ways ARIA can be applied to enhance accessibility. When used thoughtfully, attributes such as aria-expanded and aria-labelledby help communicate structure, relationships, and behavior to assistive technologies.

## Applying ARIA to enhance dynamic and custom components

Mastering ARIA means knowing its purpose and limits. ARIA isn’t a substitute for semantic HTML – it’s a supplement for when native elements fall short. Here’s how to make the most of ARIA:

- **Know when to use it:** Follow the five rules of ARIA to ensure you’re enhancing accessibility, not hindering it.
- **Check the specs:** Always consult the latest ARIA guidelines to apply roles, states, and properties correctly.
- **Communicate changes:** Use ARIA attributes such as aria-expanded and aria-labelledby to communicate states and relationships between elements. These attributes – and others like them – help assistive technologies interpret dynamic behavior and provide meaningful labels for interactive controls.
- **Test for compatibility:** Validate ARIA implementations with assistive technologies to confirm they work across devices and browsers.

By thoughtfully applying ARIA, you can ensure that custom components are accessible, intuitive, and inclusive for all users. However, writing accessible code is only part of the process; you also need to verify that it works as intended. The next step is testing your code to catch accessibility issues early and ensure your implementation holds up in real-world scenarios.

## Testing your code for accessibility conformance

One of the most essential best practices in *developing accessible websites* is learning how to test your own code. This chapter has focused on HTML, CSS, and ARIA, but even the most carefully written code won’t result in an accessible experience unless it is tested. Accessibility is not just about writing perfect code. It is about achieving conformance with the WCAG and delivering the best possible user experience for the widest possible audience. And that starts with knowing how to test your work in real-world conditions.

Developers must understand the guidelines they are designing and coding for. Testing should not be left solely to auditors or quality assurance specialists. If you’re building digital products, testing for accessibility conformance should be part of your development workflow from the beginning, not a final step. This means understanding how to identify common barriers, interpret WCAG requirements, and recognize when something that looks fine may not be usable for everyone.

A foundational accessibility testing process should include both automated tools and manual testing techniques. Automated tools are helpful for catching clear-cut errors, such as missing alternative text or insufficient color contrast. However, many accessibility issues, such as incorrect focus behavior, mouse-only interactions, or misleading link text, can only be caught by testing manually.

The manual process must include the basic use of screen reader technologies. You don’t need to be a screen reader expert, but you should be familiar with how screen readers work, how to navigate a site with one, and how your content is announced to screen reader users. Whether you’re using NVDA on Windows, VoiceOver on macOS or iOS, or another tool, this knowledge helps you catch critical usability issues that automated scans will miss.

It is also important to test your websites and components on multiple devices. User experiences vary across operating systems, browsers, screen sizes, and input methods. A design that works well on a desktop browser may break or become unusable when zoomed on a mobile screen or viewed in high contrast mode on a tablet. Testing across real environments ensures your work is inclusive by design – not by assumption.

Browser developer tools play a key role in this process. The accessibility tree, focus order inspection, and role and name checks available in Chrome DevTools and Firefox are indispensable for understanding how assistive technologies interpret your code.

All together, you should build up an accessibility testing toolkit – this is not just a collection of tools but a core part of how you work as a developer. The goal is not to use everything available but to consistently use the tools that align with your workflow, support your responsibilities, and help you build for real users.

Every developer should maintain a toolkit that includes both automated and manual testing methods. This means checking for issues with the accessibility tree, validating semantic structure, inspecting color contrast, and simulating user experiences with assistive technologies. Testing should be an integrated part of development, not a final task or something handed off to someone else.
