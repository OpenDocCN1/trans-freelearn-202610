# Links

You might use a link as part of a sentence in a paragraph. This gives users a quick way to visit another page and learn more information. In this case, don’t use phrases such as “Click here” or “Learn more” for the link text in your sentence.

For starters, not all users click with a mouse. But also screen reader users may use shortcut features that extract a list of all the links on a web page. This takes them out of the context of the sentence, so if you’re using generic terms such as “Learn more” all over the page, the user will simply hear a long list of “Learn more” links without any differentiation between them. Instead, create a hyperlink by using the words that describe the subject of your sentence or paragraph. For example, with the sentence “Learn more about our accessibility features,” hyperlink the words “accessibility features,” or make the entire sentence a link.

# Labels for buttons and interactive controls

Vague or overly complex labels on buttons (or any interactive controls) can be confusing to all users, but especially those with cognitive disabilities and non-visual users who rely on screen reader output. Labels should tell the user instantly what the controls do, how they function, and the expected outcome.

As per the guidance for links, avoid using “Click here” or “Learn more” for button labels if you can. If you do so, you must provide the developer with an extended but concise description for screen readers – for example, “Learn more about the Tier One Dental Plan” using aria-label. Or, aria-labelledby, which uses existing content on the screen, such as a heading.

# Form field labels and instructions

When creating forms, you want to make sure the form itself has a description and the form field labels and instructions are as accessible as possible. Here are some best practices for creating labels:

+   - The form itself should have a name or description, i.e. “2025 Cast Audition Form.” Developers will include this for screen reader users using either `aria-label` or `aria-labelledby`.
+   - Form field labels should be concise, specific, and placed near the input field.
+   - Use sections and headings to organize information logically. As a writer, you can work with the UX/UI designer on the structure.
+   - Indicate required fields clearly using an asterisk (*) and ensure the developer codes the form field using `aria-required`, so the word “required” is announced to screen reader users. Conversely, if most form fields are required, too many asterisks can be