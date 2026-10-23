
Designing content that works seamlessly with voice-activated assistive technologies involves ensuring your interface is accessible and responsive to voice commands. This is achieved by:

- **Using semantic HTML:** Properly structured HTML with clear headings, labels, buttons, and links makes it easier for voice technologies to interpret content accurately.
- **Providing descriptive labels and alt text:** Ensure all interactive elements (like buttons and form fields) have descriptive labels that exactly match – or at least include – the visible text label if present. This alignment helps users issue accurate voice commands (e.g., “Tap Submit”) and ensures that assistive technologies can correctly associate the spoken command with the intended element. For images, use descriptive alt text or mark them as decorative to ensure accessibility for screen readers and other voice-activated tools.
- **Ensuring consistent navigation:** Predictable and clear navigation structures help voice commands work smoothly. Users should be able to reach every part of your interface using simple, consistent voice commands.
- **Testing with voice-activated tools:** Regularly testing your design with popular tools like Google Assistant, Siri, Alexa, or Dragon NaturallySpeaking ensures compatibility and usability.
- **Providing keyboard accessibility:** Since many voice-activated tools interact with content by simulating keyboard commands, ensuring full keyboard accessibility is essential.

While users can sometimes work around poorly designed content, ensuring your site follows these principles makes their experience faster, smoother, and more intuitive.

## Identify controls

One consistent need of both voice-activated assistive technologies is to identify controls that users would like to interact with. This works differently between the two ecosystems:

- **iOS:** Voice Control expects very structured accurate commands with labels that identify the control precisely.
- **Android:** Voice Access accepts less precise commands and will attempt to guess which control is intended when a precise label isn’t available.

Because of this, iOS has stricter expectations regarding control labeling. However, it also provides developers with tools to manage this, such as the ability to separate accessibilityLabel (used to identify the control) from accessibilityValue (used to convey its current state or content). For example, a button to apply a discount code might have an accessibilityLabel of “Apply Discount” and an accessibilityValue of “Code SAVE20 entered.” This allows voice control users to say “Tap Apply Discount” while still getting feedback about the current code without confusion. Be sure to keep the accessibilityLabel concise and action-focused to serve as a reliable identifier.

## Avoid nesting controls

Both Voice Access and Voice Control activate elements by sending a virtual tap to the center of the control. This generally works well, however, in certain circumstances, the center of the control isn’t actually the correct target. If another control is nested on top of the control being activated and happens to be located at the central point, you can make it impossible for the user to activate the encompassing control.

An alternative to nesting interactive targets is utilizing custom actions. By naming actions within a component, you can avoid having several interactive targets while allowing users to use grammar like “Show Actions for {Control Identifier}.” This allows you to have several actions associated with a control while avoiding the nested controls problem.

In addition to custom actions, other effective methods for designing accessible voice interfaces include:

- **Grouping related controls:** Instead of treating each control as a separate target, group related items under a single, clearly identified control. This reduces the number of interactions required and simplifies navigation.
- **Providing contextual feedback:** Ensure that users receive immediate and relevant feedback when issuing commands. This helps confirm their actions and guides them if they need to try again.
- **Ensuring consistent terminology:** Use consistent phrasing and naming conventions throughout your interface. This minimizes cognitive load and makes interactions more intuitive.
- **Supporting error recovery:** Provide clear instructions or hints when an unrecognized command is issued. Allowing users to ask for available actions (e.g., “What can I do here?”) can significantly improve the experience.

These methods help create a smoother, more intuitive experience for users who rely on voice interaction.

While designing effective voice interfaces is essential, accessibility also depends on how well your application adapts to different devices and screen sizes. Mobile users engage with apps on phones, tablets, watches, and other devices, each with unique display characteristics and interaction patterns. To provide a consistent and accessible experience, it’s crucial to ensure your layouts are responsive across all devices.

## Ensuring responsive layouts across devices

One of the biggest challenges mobile developers face is designing for the sheer variety of devices in use, ranging from compact phones to tablets, watches, and linked external displays. Compounding this is the need to support dynamic display scaling and user-adjustable text sizes, both of which are critical for accessibility.

Supporting a broad range of devices isn’t just a technical requirement – it’s key to creating inclusive experiences. For example, WCAG Success Criterion 1.4.10 (Reflow) requires that content can be viewed without loss of information or functionality when resized up to 400%, which corresponds to a 320px-wide viewport in portrait orientation. Similarly, WCAG SC 1.4.4 (Resize Text) requires that users can resize text up to 200% without assistive technology and without breaking layout or usability.

According to Test Sigma, the most common mobile screen resolution in 2023 was 360×800 (11.02% market share), followed by 390×844 (7.93%) and 414×896 (5.56%). But successful design isn’t about hitting specific resolutions – it’s about supporting flexible, responsive layouts that adapt gracefully to any screen size or zoom level.

In spite of this, users expect support across all their devices, and you have little control over which devices your application will run on. This complexity is amplified on Android, where manufacturers like Samsung often ship their own browsers and default screen readers, potentially affecting accessibility behaviors. On Apple devices, the distinctions between iOS and iPadOS – while fewer – can still have meaningful impacts on assistive technologies and layout behavior.

## Adaptive layouts

As you start testing your applications across different information densities, you will certainly come across a situation where your application doesn’t function well in one. In these situations, it can be helpful to allow your application to use space differently based on the device it is installed on. There are a few common techniques for this:

- Adding a column or changing the number of columns in a multi-column layout.
- Having a portion of your application permanently visible, such as a menu on the left.
- Splitting your application into multiple panes, such as in a master detail workflow.

You can also consider custom design solutions with adaptive layouts, however, it’s important to remember that your developers will need to account for things that they can get for free from platform layout containers in the above methods. This can lead to inconsistencies and bugginess in UI code that is difficult to get completely correct. This balance of achieving your design goals while utilizing platform tools is important to consider early in your design process.

## Utilize standard tools

The final tip is to leverage the learnings of the platform creators. Apple and Google dominate the ecosystem because they have done their research well and know their platforms and their users. They also hold a lot of control over the future direction of their platforms and make every effort to provide backward compatibility, particularly for those aspects of their ecosystem documented in their respective design guidelines:

- Apple: Human Interface Guidelines (https://developer.apple.com/design/human-interface-guidelines)
- Android: Material Design Guidelines (https://m3.material.io/)

By leveraging the knowledge and components in these documents, you save time and maintenance pain by utilizing system components and designing applications that fit what the platform already does. Going too far outside of this bubble will leave you struggling to keep up with an ecosystem that is driven to provide users with new experiences on a yearly basis.
