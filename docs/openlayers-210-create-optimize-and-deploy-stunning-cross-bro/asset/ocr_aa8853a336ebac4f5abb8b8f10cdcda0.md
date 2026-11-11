# Panel functions

The Panel class has just two functions that we need to know about for now.

+   - **Activate()**: This function will activate the panel control itself. By default, the `autoActivate` property is set to `true` and this function will be called automatically. However, if the panel is not activated by default, you must call this function to use the panel.
+   - **activateControl({OpenLayers.Control})**: This function will activate the passed in control. The control passed in must be a control that belongs to the panel. This is the function that gets called when a control inside the panel is clicked on by a user. If you wish to manually activate a control, you will call this function.

# Now what?

Whew! We've covered the majority of controls OpenLayers offers and we've gone over how to use a panel. There's been a lot to take in, but don't feel overwhelmed. One of the reasons we covered so much is so that we can refer back to this chapter later if we need a refresher on how to use a control or what we can do with one.

So, now that we're done with the nitty gritty details, let's move on to something a little bit more complex.

# Creating our own controls

You're familiar (or perhaps fed up) with control descriptions by now. Let's make things a little more interesting and create our own control. Here's the plan:

+   1. Quickly talk about the Button Control class.
+   2. Create a Button control with custom functionality.
+   3. Add the button to a panel.
+   4. Viola! You just created your own custom control.

So, before we talk about creating a custom button, let's go over the **Button** subclass that we didn't cover in the section above, which we'll use to create our custom controls.

# OpenLayers.Control.Button

There's not much to cover here really, as we've been over the base control class. There are two properties we need to discuss however—the `type` and `displayClass` property. We'll use these two properties when creating our own custom control.