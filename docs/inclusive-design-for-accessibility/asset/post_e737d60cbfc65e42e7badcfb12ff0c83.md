
As you navigate your way through, pay attention to how the screen reader describes each element in focus. Screen reader announcements should be helpful, not hectic or absent; they shouldn’t be repetitive or point out any unnecessary junk.

> If this is your first trip to screen reader city, you may notice it speaks fast. Don’t worry, you can adjust the speed in your screen reader’s settings. There’s no shame in slowing things down at first. Most daily users use their screen reader at full speed, but if you don’t often use one, slow it down to a more comfortable speed.

For interactive elements, such as buttons or links, the screen reader should announce the label followed by the type of element (called its “role”). For example, a button with a text label of “Add to cart” should be announced as “Add to cart, button.” A big no-no would be if the visual label says “Add to cart” but the screen reader announces it as “Buy it now, checkbox,” which can confuse or mislead users.

Additionally, if the accessibility name does not match an element’s text label, it may cause issues for users who navigate by voice. When navigating by voice, the person selects elements by saying the visible labels. If the visible label differs from the accessibility name, the assistive technology may not recognize the element or respond to the voice command. Technology advancements may improve this soon though, so follow the latest operating system and software updates.

Also, look out for other common screen reader announcement issues, such as the following:

- Missing role or element type (i.e., not saying that a button is a button).
- Decorative images are not ignored.
- Improper use of headings (i.e., illogical order or not descriptive).
- In-line error messages are not announced.
- Form fields lack required or optional indications.
- Status messages are not announced. For example, if a form is submitted, there should be a success message indicating it was submitted or saved.
- Focus on management issues that impact the user experience.

## Mobile device-specific screen reader tips

Turn on your screen reader in your device’s accessibility settings (or ask the default voice assistant), and then use gestures to test a mobile application. Gestures are a common way to navigate a mobile device with a screen reader.

Most devices provide a quick tutorial of common gestures when you turn on the screen reader for the first time. Once the screen reader is running, you can use these gestures.

Common gestures include the following:

- **Swipe right:** Move the focus forward
- **Swipe left:** Move the focus backward
- **Single tap:** Select an item
- **Double-tap:** Activate an interactive element in focus
- **Swipe with three fingers:** Scroll the page in the direction of your swipe (iOS only)
- **Swipe with two fingers:** Same as above but for Android

Using the screen reader and gestures, check that the focus order, screen reader announcements, and core functionality behave as expected.

## Check descriptions of images

Automated tools are great at flagging that there’s a missing image description – often referred to as **alternative text (alt text)** – but they can’t judge whether the text is helpful or accurate. That’s where manual testing comes in.

Assistive technology should describe meaningful images in a way that communicates the purpose. One way to test alt text is by navigating to the image and observing the screen reader or braille output. Listen to (or read) what’s announced and ask yourself: Does this make sense? Does it add value without being overcomplicated?

Pay attention to the length of the image description – it should be detailed but concise (no one needs a novel). If the image contains important text, make sure that text is included in the alt description. If the image is just decorative, the screen reader shouldn’t even mention it.

Want to double-check the alt text on a website? Pop open your browser’s developer tools, inspect the element, and look for the alt="" attribute in the HTML code. The alt text is what is between the quotation marks. If the image is purely decorative, the code should still have alt="" but no text between the quotation marks. This tells assistive technologies, “Hey friend, you can ignore me. I’m just here for the visual vibe – nothing more, nothing less!”

## Test the page in various states

Don’t stop there. There is still more to explore! Are error messages, loading indicators, and changes in content (such as expanding an accordion to reveal text) announced? If not, the experience leaves much to be desired and could leave many end users feeling lost or frustrated.

It’s important to manually check the behavior of dynamic content because most automated tools only test the page in the state in which it loads. So, for example, it may not test menus or accordions when they are expanded, when in-line error messages are present, or if a date picker appears after selecting an appointment time.

While navigating with a screen reader and keyboard, explore these additional states. When you encounter a menu, confirm that you can reach all options in the menu without using a mouse and that the screen reader accurately announces all the options.

## Zoom in on what matters: your product

People with low vision or other disabilities may use features that enlarge digital text or content. That is why it’s important to test your experience at various zoom levels and text sizes. Pinch to zoom on mobile devices or increase the web browser’s zoom level to ensure all content reorganizes without overlap, cutoffs, or decreased functionality.

Then, test with large text. Adjust your text size in your device or browser settings and then verify that all text resizes proportionately, no content overlaps, and no functionality is lost.

## Color counts

Color vision deficiency is quite common. So, make sure color alone is not used to give instructions or convey meaning. For example, errors in a field should not only be marked in red; they should also have an inline error message announced by a screen reader. That doesn’t mean you can’t use color, you just shouldn’t use only color to indicate meaning.

Also, make sure your digital experiences use sufficient color contrast. Most times, automated tools can catch color contrast issues; however, some may slip by. If you can do a quick visual scan of the page, look for any low-contrast text and non-text elements. If anything on your page looks like it went through the wash too many times (i.e., faded), it’s time to chat with your designers.

## More essential checks

There are several other things to check manually:

- Easy and correct heading structure
- No quick flashing content
- Consistent navigation
- Animations and videos have a way to pause and play
- Videos have captions
- Audio comes with a transcript
- Session timeout warnings, letting users extend their session

Let’s finish by looking at some mobile-specific considerations:

- **Orientation support:** Devices can be used in either portrait or landscape mode. This relates to accessibility because some people may have their phone or tablet in a fixed position, such as mounted to a wheelchair. Test your digital product in various orientations to ensure support. When you turn your phone horizontally, the view of your app or mobile website should seamlessly switch to landscape view. Similarly, when you change your mobile device back to the vertical position, the content should switch back to portrait view. In both positions, the content should not overlap, cut off, or lose functionality.
- **Built-in accessibility features:** There are many accessibility features available on mobile devices, so it’s important to test how they work in your app or mobile website. These features are typically available under **Device Settings** or **Accessibility Settings**.

Some features have slightly different names on iOS versus Android. Refer to the following table for several examples.

| Feature | Android name | iOS name |
|---|---|---|
| Voice navigation | Voice Access | Voice Control |
| Text-to-speech | Select to Speak | Spoken Content |
| Screen reader | TalkBack | VoiceOver |
| Increase text size | Font Size | Larger Text |
| High contrast | High Contrast Text | Increase Contrast |
| Minimize animations | Remove Animations | Reduce Motion |
| Alternative input | Switch Access | Switch Control |
| Enlarge screen content | Magnification | Zoom |

*Figure 11.3: Names of iOS and Android features*
