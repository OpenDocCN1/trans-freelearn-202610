Using Tailwind CSS, the form has been styled as shown in the following figure:

# # Add an Event

![](bbox=[0.13, 0.17, 0.85, 0.57])

Figure 13.1 – The event proposal form styled with Tailwind

> **Note**
> The full documentation for Tailwind CSS can be found at https://tailwindcss.com.

For the purpose of this book, we’ll look at the following example, which is simply one small component of the full capabilities of Tailwind CSS. Here, we have the previous HTML code block now styled with Tailwind:

```
```html
<div className='mt-10 block'>
  <label className='label'>Event Title:
    <input className='form-input mt-1 block w-full' type='text' name='eventTitle'/>
  </label>
</div>
```
```