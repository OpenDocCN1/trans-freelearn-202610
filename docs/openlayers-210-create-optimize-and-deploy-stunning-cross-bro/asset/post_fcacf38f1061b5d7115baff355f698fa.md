
2. First, we're going to create the function that will randomly change the map's base layer's opacity when the map is clicked. This will only happen when our toggle control is active. Right before the `map.addControl(control_panel);` line, insert the following:

```javascript
//Function that the map will call when the map is clicked (only when
// the toggle button is active though)
var map_event_function = function(){
    map.layers[0].setOpacity(Math.random());
}
```

3. Before we create a `TYPE_TOGGLE` button, we need to create two functions: one that will get called when it is activated and one called when it is deactivated. This is similar to what we did in the previous example. Let's do that, right after the previous code:

```javascript
//Create a function for the toggle button
var toggle_button_activate_func = function(){
    //Attach the map_event_function to the map
    map.events.register('click', map, map_event_function);
}

var toggle_button_deactivate_func = function(){
    //Remove the map_event_function from the map
    map.events.unregister('click', map, map_event_function);

    //Restore the layer's opacity
    map.layers[0].setOpacity(1);
}
```

4. Now, we need to create the toggle control itself. The format is similar to before, but we'll be using a couple different properties. Because this is a toggle control, we won't use a trigger function. Instead, we'll add two event listeners—one for the activate event, and another for the deactivate event. We'll also set the type to `OpenLayers.Control.TYPE_TOGGLE` (the `TYPE_TOGGLE` type—we could also set the type to 2 to accomplish the same thing, but it's much more readable to use the built in constant name). Add the following on the next line:

```javascript
//Create the toggle button object
var custom_toggle_button = new OpenLayers.Control.Button({
    displayClass: 'olControlCustomButtonToggle',
    eventListeners: {
        'activate': toggle_button_activate_func,
        'deactivate': toggle_button_deactivate_func
    },
    type: OpenLayers.Control.TYPE_TOGGLE
})
```

5. You can probably guess the next part—add the control to the panel. Easy enough!

```javascript
control_panel.addControls([custom_toggle_button]);
```

6. Now, just make sure the control_panel is added to the map:

```javascript
map.addControl(control_panel);
```

7. Lastly, we just need to add the appropriate CSS classes. We gave our toggle control the class of olControlCustomButtonToggle, so we'll need to add the appropriate classes. Because this is a toggle control, there are actually two CSS classes we need—one for active, and another for inactive. Add the following inside the <style> tag in the <head> section:

```css
/*Custom Toggle Button*/

.olControlCustomButtonToggleItemActive {
    background:#336699;
    border:5px solid #202020;
    cursor: pointer;
    height: 28px;
    width: 28px;
}
.olControlCustomButtonToggleItemInactive {
    background:#003366;
    border:5px solid #202020;
    cursor: pointer;
    height: 28px;
    width: 28px;
}
```

8. All done! When you click the newly created control, the activate() event is fired, and since we added an event listener for it the appropriate toggle_button_activate_func is called. It will add an event handler to the map that will wait for a click. When the mouse click event fires, the base layer's opacity will be set to some random value between 0 (fully transparent) and 1 (fully opaque). Then, when the toggle control is clicked again, it will be deactivated and the layer will return to full opaqueness.

![](img/1b8cd151c9f278555f9cf255b20d244e_187_0.png)

## What Just Happened?

You probably have a good idea now of how the controls we just made work. Just as a quick recap, the following happens:

- 1. The `custom_toggle_button` toggle control waits for the `activate` or `deactivate` events to be fired (these are custom OpenLayers events, not general JavaScript events like a mouse click). This is done by registering the events with `map.events.register`.
- 2. When the `custom_toggle_button` control is clicked (which toggles it on), the `activate_control` event is detected and the corresponding `toggle_button_activate_func` function gets called.
- 3. The `toggle_button_activate_func` function that gets called then adds an event listener to the map (with `map.events.register` again).
- 4. This event listener will wait for a `click` (a mouse click, a general JavaScript event) and then fire off the `map_event_function` every time a click is received.
- 5. When the `custom_toggle_button` control button receives a `deactivate` event (i.e., it is clicked and toggled off), the corresponding `toggle_button_deactivate_func` function is called.
- 6. The `toggle_button_deactivate_func` function removes the previously added map event listener with `map.events.unregister`. Therefore, the listener is gone and mouse clicks on the map will no longer cause the `map_event_function` to be fired off.

And that's about it!

![](img/1b8cd151c9f278555f9cf255b20d244e_188_0.png)

> When using panels and controls inside panels, make sure that the controls get activated via `activate()` or the panels' `activateControl()` methods. The control cannot be used if a control is not activated (or cannot be activated by the user by clicking on the control, for example).

## Summary

We covered a good deal in this chapter. We talked about the idea behind controls and how they are used in OpenLayers. We then demonstrated how to add controls to a map. We covered the Control class and its various subclasses in depth. We also went over how to set up panels and how to add controls to them. To finish the chapter, we provided an introduction to events and learned how to create our own custom controls.

Even though we've been pretty thorough in this chapter, there's still more to learn about controls—such as how to style them. In Chapter 9, on the Vector layer class, we'll go over some Vector layer specific controls we neglected. This chapter was likely a lot to take in, but the next chapter will be a bit shorter. We'll discuss styling controls using CSS, and we'll make controls that actually look like they should be clicked, not just big green and blue squares.

## 7 Styling Controls

*So far, we've seen how we can customize nearly everything offered by OpenLayers—Layers, Controls, etc. However, we haven't been too concerned with the actual appearance of the interface of our maps.*

*OpenLayers provides pretty good looking default controls, so you may not find it necessary to change them. Like nearly everything else in OpenLayers though, changing the style and images used by the Control class is easy.*

*We talked about the Control class in the last chapter, and in this chapter we'll explore how OpenLayers applies styles to controls and how to go about customizing controls.*

In this chapter we'll:

- Talk about what CSS is and what purpose it serves
- Discuss how HTML uses CSS
- Go over how OpenLayers uses CSS
- Cover the naming schemes OpenLayers uses for controls
- Give controls some custom styles

Let's dive in!

## What is CSS?

**CSS** is an acronym for **Cascading Style Sheets**. It is a type of markup language used to specify the appearance of HTML elements. CSS is actually quite simple, and if you've been able to follow along up to this point in the book, you will have absolutely no problem at all with CSS. Before we get into it a little more, let's talk a little bit about how CSS works.

## Ideas behind CSS and HTML

**HTML**, **CSS**, and **JavaScript** serve three distinct purposes.

- **HTML** is used, as we've seen, to create the structure and *content* of a webpage
- **CSS**, on the other hand, is used to control the site's *presentation*, or how the page should look
- **JavaScript**, as we've seen throughout the book, is used to handle the *logic* behind the site

So, when you make a site you will have at least three discrete things to consider—the HTML behind it, the CSS that styles the HTML, and the **JavaScript** that handles any logic or user interaction. For now, we'll focus on HTML and CSS.

Ideally, HTML, CSS, and JavaScript should not mix (i.e., you shouldn't use style tags, or tags like `<center>`, in your HTML). Your HTML pages should link to external JavaScript and CSS files, especially in a production environment. One advantage of following this principle is that if you want to change the way your website looks, you only have to edit the CSS in one place (the external file) instead of on every page. The disadvantage is that it is slightly easier and faster to edit things if everything is contained inside the same HTML file; that's why we've been doing it this way so far.

But how do we edit the CSS and what does this even mean?

## Editing CSS

If you're familiar with CSS, feel free to skip the next few pages and go directly to the second section on *How OpenLayers uses CSS*. If you aren't, don't fret! It's quite easy.

CSS isn't a programming language; in fact, you already are familiar with how it is structured. Let's look at the 'template' code for applying a CSS style:

```
element {
    property: value;
    property-two: value;
}
```
