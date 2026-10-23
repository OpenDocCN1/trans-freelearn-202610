
## Use plain language in plain sight

First of all, you should recognize that people aren’t always rich in time, attention, and focus to consume, search for, understand, and effectively use your products. Anyone can be affected by a hectic schedule, a noisy environment, or drowsiness from a cold medicine. Likewise, people with cognitive differences may need extra time to complete tasks or recall information.

If people can’t find, consume, and understand the content, they can’t use your product. Through style guides and design pattern examples, encourage teams to avoid jargon, idioms, marketese, and specialty phrases.

Discourage consuming teams from hiding important information behind tooltips, dialogs, or expanders. Hidden information will not be as readily consumed as useful information placed in visual proximity to decisive UI features.

## Affordances for text inputs

Cognitive load increases for all people when all text fields are the same size. Instead, define rules for sizing text fields based on the expected input.

For example, if a field asks for the birth year, the field should be wide enough to contain four characters, but not more. This gives all people the ability to intuit the function of controls and input fields at a glance, both increasing the speed at which everyone can fill in form data while simultaneously reducing errors.

## Encourage autocomplete

People invented computers to do repetitive work, and inputs using autocomplete attributes allow just that. When forms don’t use or allow autocomplete, the friction to filling in forms correctly increases for all people. For people with motor disabilities who are using switch devices or alternative navigation, this can be an especially tedious and frustrating barrier.

## Discourage distractions

People with difficulty focusing need distraction-free interfaces. For them, an autoplay video or background animation that cannot be quickly paused can mean a UI is unusable. Meanwhile, popups trying to upsell in the middle of a transaction can cause irritation (even for those who have no problem focusing), and push users elsewhere. To solve this, it’s simple: if it’s annoying, it’s not accessible. Set strict guidance on how dialogs, modals, and alerts can be used.

By prioritizing plain language, sensible input design, and minimal distractions, design systems can foster a more inclusive experience that empowers people with cognitive disabilities to participate fully.

## Motor disabilities: designing for imprecise actions

Design can accommodate people with motor disabilities who simply have difficulty precisely interacting with the UI but aren’t using other assistive technology.

It’s not difficult to understand that bigger buttons are easier to click on for everyone. People with difficulty using a mouse or tapping their screen with precision will encounter barriers when buttons are too small or packed too closely together.

To help, encourage mobile-first design with generously sized controls. While it’s common for consuming teams to feel pressured to make densely featured interfaces, the design system examples and documentation must provide examples supporting an accessible target size.

To meet minimal WCAG AA target size requirements, objects must be 24x24 px in size with a 4 px margin. A more generous size of 44x44 px meets WCAG AAA and HIG requirements, and Android Material Design calls for a 48x48 dp minimum size.

![](img/3bdc9e2d41de898f8a3bcc9634b0000e_252_0.png)

Figure 10.13: WCAG AA minimum target size and AAA target size

For HTML-based controls such as checkboxes, radio buttons, expanders, and switches, the entire label must also be clickable, not just the control itself.

![](img/3bdc9e2d41de898f8a3bcc9634b0000e_252_1.png)

Figure 10.14: Web-based controls such as switches must activate when clicking on the label and the control itself

By designing generous touch targets and ensuring entire labels are clickable, teams can remove barriers for people with motor disabilities – creating interfaces that feel intuitive and usable for everyone. A design system that champions these affordances not only meets accessibility standards but also delivers a more inclusive experience.

## Motion sensitivity

People with vestibular disorders can be made ill by sweeping animations or videos. Designs using parallax scrolling or auto-playing full-screen videos can literally make someone sick for hours, causing dizziness, nausea, or disorientation.

### Respecting reduced motion settings

Start by documenting how autoplay video and animations must respect the device’s **Prefers Reduced Motion** settings. This system setting, available in most operating systems, is detectable in browsers and by native mobile applications, indicating a preference for less motion or animation. For example, when a user has enabled this setting, design systems should disable parallax effects, slow down or remove transitions, and pause automatic animations unless user-triggered.

### Plan ahead for changes to motion

The design system must document how video or animation will react to being turned off. For example, if a hero banner features a looping background video, provide a static fallback image with similar branding or messaging. For micro-animations, replace them with subtle fades or highlights that convey the same meaning without inducing motion sickness.

### Feature easy-to-find controls

Any autoplay videos must include intuitive, easy-to-find controls for pausing or stopping playback. Avoid burying these controls in nested menus – ideally, position them near the video itself with clear labels such as **Pause animation** or **Stop motion**. For background animations (e.g., moving gradients or background effects), offer toggles or switches so users can easily disable them if needed.

Design systems that respect motion sensitivity build trust with users by prioritizing comfort and well-being. By offering flexible motion settings, fallback visuals, and easy-to-find controls, teams create interfaces that empower people with vestibular disorders to engage safely and confidently.

## Photosensitivity

People with photosensitivity carefully manage their exposure to digital environments to reduce the risk of seizures or illness. Triggers include flashing lights (such as strobe effects) or certain high-contrast visual patterns, such as stripes, grids, and checkerboards. For some, even subtle flickers can cause dizziness, headaches, or disorientation.

### Set clear guidelines

The design system must establish clear guidelines for all animations and videos, ensuring that any content qualifying as flashing does not exceed three flashes per second. This aligns with WCAG 2.2 Success Criterion 2.3.1 and helps protect users with photosensitive epilepsy.

### Provide warnings and options

Video or animation features must consider where to intuitively place warnings within the interface if photosensitive content is present. Placement will naturally vary depending on the platform – web, smartphone, tablet, or TV – but should always be consistent and easy to find.

Plus, you should also:

- Include a clear, readable label such as **Warning: Flashing content may affect photosensitive users.**
- Offer toggles to opt in or out of high-motion effects. For example, allow users to disable parallax scrolling, background videos, or dynamic gradients.

### Test for safety

The design system should document how to test content using automated tools such as the **Photosensitive Epilepsy Analysis Tool (PEAT)** software from the Trace Center at the University of Maryland (https://trace.umd.edu/peat/). Additionally, consider collaborating with accessibility specialists or user research participants who have photosensitivity to ensure a real-world, lived experience perspective. Work with users with photosensitivity to understand their experiences and needs, and validate solutions.

### Additional tips

As well as the previous information, consider these tips:

- Avoid high-contrast flashing between light and dark colors
- Test not only isolated features but also complex interactions, such as transitions that combine multiple visual effects
