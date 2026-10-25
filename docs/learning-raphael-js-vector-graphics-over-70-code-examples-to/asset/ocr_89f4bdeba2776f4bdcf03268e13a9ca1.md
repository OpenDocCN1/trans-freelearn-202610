# # 4 Transformations and Event Handling

Having spent the past two chapters drawing static graphics, we will now start to look at how we can transform them and how we can interact with them in the browser.

When you create an element in Raphaël, you are effectively creating a **Document Object Model (DOM)** object. Like the DOM objects you are already familiar with elements drawn on to our canvas will trigger events when interacted with by a user. Raphaël provides a number of convenient, cross-browser methods for interacting with elements and also provides methods that facilitate the drag-and-drop functionality.

Transformations are equally versatile: Raphaël gives us the ability to transform (translate, scale, and rotate) elements with ease using the concept of a **transformation string**.

Over the course of this chapter, we will look at event handling and transformations alongside each other, combining both facets of the library to yield interactive graphics. Overall, we will cover:

+   - The transform method and basic transformations
+   - Matrix transformations
+   - Registering and unregistering basic event handlers
+   - Achieving the drag-and-drop functionality