
- **Informative images** are ones that convey meaningful content or information within them. A common example is step-by-step photos or illustrations on a recipe website, showing how to make a cake from scratch. The purpose of these images is to convey information in visual form. They need alternative text to describe them for non-sighted users. There may be text on the page or screen that relates to the image, but alt text should still be used to describe exactly what’s in the image. Also, the alt text should not simply repeat contextual text around it that says the same thing.
- **Functional images** are images used to initiate actions, rather than to convey information. They are used in buttons, links, icons, and other interactive elements. For this reason, the alt text should describe the function, not the image itself—for example, a print button uses only a print icon to visually convey its purpose. Its alt text would be “Print” not “Printer.” Similarly, the search icon is a functional image for a button, labelled “Search” not “Magnifying glass.”
- **Complex images** are informative images that provide a lot of detailed and sometimes interconnected information or data points. These certainly are not just decorative. The data is usually displayed in a highly visual format such as pie charts, bar charts, diagrams, or infographics. There can be text mixed with color-coded lines, symbols, and shapes. In this case, the information is presented in a non-linear fashion, which makes it difficult for screen readers to navigate and share the information in a meaningful sequence to users with visual impairments.

Let’s take a closer look at alt text, and why it is important. We will also look at why we should avoid using text in images.

## Utilizing alt text

Working with a designer, you may choose images, photographs, or illustrations to accompany your writing piece. But if you do use them, it’s your job as a content creator to transform their subject, purpose, and emotions in a way that supports people with blindness, limited vision, and/or those with cognitive needs who may use a screen reader to convey the image’s meaning to them.

Alternative text, or “alt text” for short, is a textual substitute for non-text content in web pages, mobile applications, and documents. Effective alt text is meaningful and descriptive of the image, animation, or video, and its subject and actions.

If you don’t provide content for the alt text tag in HTML, the screen reader will announce either the image filename (a brutal series of numbers and letters perhaps!) or nothing—only “image.” This only tells the user they’ve encountered an image on the screen.

Here are some considerations when it comes to alt text:

- Use alternative text to describe meaningful images concisely.
- Provide detailed descriptions for complex images via captions or linked text.
- Ensure text alternatives convey the same information as the image.
- Ask the developer to mark decorative images properly to avoid unnecessary distractions for screen reader users. This means that you won’t add a text alternative as it’s not necessary.
- If text on the page provides enough information without the image, mark the image as decorative. But if the image has its own meaning, add alt text.

![](img/3bdc9e2d41de898f8a3bcc9634b0000e_159_0.png)

> There’s a misconception that all images need a description, but that is the strength of a content author to say when it is and isn’t necessary. Decorative images don’t need alt text, if they’re just providing a visual for presentation style. The WAI alt Decision Tree is a popular tool that content authors use. It provides a series of yes/no questions to help you decide whether alt text is needed for an image: https://www.w3.org/WAI/tutorials/images/decision-tree/.

It’s also worth considering graphs, charts, and icons in the context of alt text.

## Graphs and charts

Graphs and charts fall under complex images and so need alt text. But here are some specific considerations:

- Provide text-based summaries of key insights and data relationships. This assists screen reader users who may not be able to make interpretations and analyses just based on the diagrams.
- Label axes and data points so it is clear what the diagram is showing. Ultimately, this benefits everyone!
- Offer alternative data views – for example, the same information, but in a table. As discussed, people with cognitive needs sometimes prefer a data table with clearly defined relationships across a recognized block of information. This can be easier to follow, rather than multiple lines and color-coded visuals on a chart.

## Icons

Icons are uniquely visual and can offer your target audience instant recognition of your chosen subject, if conventional ones are used. For example, the help icon is often a question mark, while the search icon is often a magnifying glass.

What’s interesting is that icons stand alone without a visible content label a lot of the time. That’s by design, but the most inclusive approach would be to combine an icon with a text label that can be read by a screen reader.

Now, do keep in mind that the label should describe the icon’s function, not a description of the icon itself. As mentioned, if you use the magnifying glass icon, the alt should be “Search” (not “Magnifying glass”).

Here are some more universal icons and the recommended alt text:

- Magnifying glass icon: “Search” for a search input field
- Shopping cart icon: “Shopping cart” or “View cart” in a transaction flow
- House icon: “Home” for a link or button that returns the user to the homepage
- Envelope icon: “Email address” or “Contact us” for a link or button that opens an email agent or directs the user to a web page with a contact form
- Phone icon: “Call us” next to a link of a phone number that launches a mobile phone call
- Printer icon: “Print this page” on a button that launches the native print function in a browser
- Download arrow icon: “Download [file name]” as a button, or next to a link, to launch a download on desktop or mobile
- Trash can icon: “Delete” or “Remove” on a button or next to a link that launches this function
- User profile icon: “Profile” or “Account settings” on a button or next to a link that opens a new page or popup with profile information and/or account settings
- Gear or wrench icon: “Settings” on a button or next to a link that launches a page or popup to change settings within the website or web application, or evoke native app settings.

Include text labels alongside icons when you can, and encourage the visual designer to leave room for this on the page or screen. This effort supports all users, but especially people with low vision, learning difficulties, and neurodivergence.

Sometimes, designers will place an icon in front of a text link to make it stand out on the page, or provide a clear visual indicator. A good example is an email icon next to an email link. As long as the link text is descriptive and meaningful of its purpose or destination, there’s no need to add alt text to the icon. It can be defined as decorative only.

One exception to this is the icon that designers will place before or after a link to indicate that the link opens the page in a new browser window or tab. This icon is an informative image, not decorative. The icon needs alt text so non-sighted users are aware that the link opens a new window. Use alt="opens new window" to do this. Alternatively, the image can be decorative, and the link itself can have an aria-label attribute with the visible text. For example, if the visible label is "Learn more", the code could be aria-label="Learn more about Mortgages. Opens new window."

## Avoiding text in images

We’ve looked at the importance of alt text, but let’s consider another image-text issue: text in an image cannot be parsed by screen reader software, so it’s completely lost to non-sighted users.

For example, banner ads are bad design. They contain a bunch of text that is often crammed into a restricted layout, making the font size small and often hard to see, especially by people with low vision. For all content on the page, mobile screen, or document, work with the UI designer and inform them to avoid using images with text embedded inside.

Instead, use real text on the page, and work with the UI designer to create a background image underneath. For the text, ask them to use system fonts such as Arial or Verdana for the most readability.

For practical reasons, sometimes it’s just not realistic to follow this guidance. Use system fonts whenever possible, but in cases where the image has text in it, that text needs to be reflected within the alt value.
