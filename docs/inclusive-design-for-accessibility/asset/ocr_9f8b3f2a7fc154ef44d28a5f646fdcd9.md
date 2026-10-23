The problem with non-semantic HTML is that it provides structure but no meaning. It’s like putting content inside an invisible box – screen readers and search engines can’t tell what it’s for, or sometimes even that it’s there. The `<div>` and `<span>` elements, for instance, don’t communicate the purpose of their content, making it harder for assistive technologies and search engine crawlers to understand or interpret the page correctly.

# Defining semantic HTML

**Semantic HTML** structures a web page using elements that inherently describe their purpose, making content more meaningful and accessible. Tags such as `<header>`, `<article>`, and `<section>` provide clear definitions of their role, allowing both browsers and assistive technologies to interpret and navigate the page effectively. This improves readability, enhances accessibility, and strengthens search engine optimization by giving content a logical hierarchy.

By using elements according to their intended role rather than visually styling non-semantic elements, developers create a well-structured and intuitive web experience. For example, `<footer>` naturally indicates closing information, while `<nav>` clearly defines a group of navigational links. These distinctions provide clarity, ensuring that users, especially those relying on assistive technology, can interact with web content in a meaningful way.

Let’s take at this code block:

```
```html
<!-- Semantic HTML: Native button -->
<button type="submit">Submit</button>

<!-- Non-semantic imitation: Recreating a button with a <div> -->
<div
    role="button"
    tabindex="0"
    onclick="submitForm()"
    onkeydown="if (
        event.key === 'Enter' || event.key === ' '
    ) submitForm();"
>
    Submit
</div>
```
```

The first example uses a semantic `<button>` element, which is accessible by default and supports keyboard interaction, screen readers, and form submission without extra code. The second example shows a `<div>` with `role="button"`, `tabindex="0"`, and JavaScript event handlers added to imitate button behavior. This approach increases complexity and risks missing critical accessibility features.