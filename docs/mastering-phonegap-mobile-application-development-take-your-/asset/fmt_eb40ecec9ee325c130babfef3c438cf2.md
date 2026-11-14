
- `Interact` ([https://github.com/taye/interact.js](https://github.com/taye/interact.js)): This supports drag and drop, resizing, and more, with inertia.
- `Zepto` ([http://zeptojs.com/#Touch%20events](http://zeptojs.com/#Touch%20events)): This is a lightweight jQuery replacement, but it has support for simple gestures, such as swipe and tap.
- `tap.js` ([https://github.com/alexgibson/tap.js](https://github.com/alexgibson/tap.js)): This is similar to `FastClick`, but it uses a tap event instead of overriding `click`.

While all of this goes a long way in reducing the perceived lag, there's still more we can do. Typically, the earlier listed libraries are useful in responding to an event once it has finished, but what if an event is in progress? For example, apps don't usually wait to register that an onscreen button has been pressed until one lifts their finger; instead they usually display a depressed button (or some other state) while the finger is on the screen. This reduces the feeling of lag and increases the perception of performance, because now we can see the app responding directly to our touch, not just to the completion of a gesture.

The easiest way to do this is simply to use the various states that CSS already provides us, namely, the `:active` and `:focus` states. While `:hover` is supported by mobile web views, it has quirky behavior and is best avoided.

## Note

`:hover`, of course, has no real analog in a touch environment, because the device isn't capable of recognizing when your finger is actually hovering over the screen. So, the browser takes a different tact: one tap on an element will transition it to the `:hover` state, and then another tap will invoke a `click` event. Although I understand the reasoning, it's terribly frustrating when I encounter it on web pages, because it means that I have to start tapping everything twice. So please, don't use it in your apps!

The `:focus` state occurs when an element receives input focus. This is valid only for `input`, `textarea`, `select`, and other similar elements. By changing the visual state of the element, even subtly, the user can be assured that the app is focused on the appropriate element. This also has benefits if the user has paired a hardware keyboard, since they can then `Tab` through the elements like on a desktop browser.

The `:active` state occurs when the element receives a touch. This is probably the most critical state to implement, because it gives nearly immediate feedback to the user, *without any JavaScript code whatsoever*. This means that it also generates the least lag. You shouldn't try do to anything *too* fancy while changing to the `:active` state, because you don't want to force the browser to recalculate the positions of every element; but changing the text color (even subtly) is usually sufficient and extremely fast.

This particular issue is most obvious when you see visual stutters while scrolling or animations are playing. The stutters occur because the device can't perform all the necessary computations to render the screen fast enough to meet the target `60 fps`. To reach this target, the device must execute *your* code, layout the `DOM`, composite various elements together, and send the result to the `GPU` within roughly `16 ms`. If it sounds like a really short time, that's because it is. For a computer, of course, this is a reasonable amount of time. But while dealing with computing the locations and sizes of `DOM` elements that are relative to each other, compositing transparent and translucent elements on top of one another, and then pushing all the data across to the `GPU`, these milliseconds quickly disappear.

If you are seeing this particular issue, the profiling tools we've already mentioned should help point out where the potential issues are lurking. This is because the profilers record how long it takes to generate each frame. Consider the following example:

![Correcting visual stutters](graphics/9781OS_10_09.jpg)

This graph is a representative of what you can see on both Google Chrome and Safari. The height of each bar indicates how long the frame took to render, and the various segments indicate where the time was spent.

As you can see in the preceding graph, our app only achieves `60 fps` when the height of the bar is below the `60fps` line. Both Google Chrome and Safari will indicate the `60fps` line. They will also show a `30fps` line if necessary.

Once our frames start taking longer than `16.67 ms`, we can't sustain `60 fps`. If we're slower than `33.33 ms`, we can't sustain `30 fps`. If this occurs infrequently, it's not such a big deal, because you're never in *total* control of the device (the operating system might be performing an install, the browser might be collecting garbage, and so on). But if it's consistent, it will be obvious in the form of visual stuttering.

## Reaching 60 fps

So, how do you tame the frames-per-second beast? Here are some suggestions:

- Keep the `DOM` simple. If an element doesn't need to belong in the `DOM`, don't add it.
- Avoid deeply nested elements in the `DOM`. Changes made to the child elements will invariably force the parent elements to recalculate their position and size, which can be a costly computation.
- Avoid long lists of `DOM` elements. Scrolling through long lists can quickly become laggy and slow, so it's better to limit the number of elements that can be presented. You can use paging to accomplish this pretty simply or you can use a variation of infinite scrolling, which is closer to what native `SDKs` use. However, the latter is much more complicated. Some infinite scrolling libraries are, as follows:
  - `Infinity` ([http://airbnb.io/infinity/](http://airbnb.io/infinity/))
  - `iScroll` ([http://iscrolljs.com](http://iscrolljs.com))
- Cache references to `DOM` elements. Typically, you will need to use some sort of `CSS` selector rule to locate the `DOM` elements you need. This can be expensive, especially if the `DOM` is nested to any degree. Looking up elements by `ID` is the fastest option, but you may not always have this option. So, instead of searching the `DOM` repeatedly when you don't need to, cache the results.
- Avoid unnecessary `DOM` reflow. This occurs whenever the browser needs to calculate the position, size, and styling of elements, usually in response to either a query for a specific property (such as `window.getComputedStyle(element).width`), or when various styles change (like `element.style.width="100%"`). Adding elements to nodes can also cause the `DOM` to reflow, which means that you should avoid using `parentElement.appendChild(element)` in a loop where `parentElement` is in the `DOM` (Use `document fragments` for this purpose.) A reflow also occurs while removing elements, so it is often faster to hide an element (`visibility: hidden`) in comparison to removing it, especially if you know you'll need the element in the future (as in the case of a sidebar menu).
- Keep your `CSS` pruned. The more complicated your `CSS` is, the more complicated it is to compute the various properties on your `DOM` elements. Remove unneeded rules and avoid complex selectors.
- Keep your `CSS` layouts simple. It's important to remember that the browser is actually solving some very complex equations in order to satisfy your layout requirements, and these can add up quickly. For example, `Flexbox`, while easy to use, is also difficult to compute. You can avoid some of the computations by positioning elements absolutely or using `CSS transforms`, but this can also remove the benefits of `HTML` and `CSS`: apps that are responsive to the viewport's size. If you position everything absolutely, it will be incumbent upon your own code to recalculate every element's position. Depending on the structure of your app, it might actually turn out to be more time-consuming than letting the browser handle this responsibility.
