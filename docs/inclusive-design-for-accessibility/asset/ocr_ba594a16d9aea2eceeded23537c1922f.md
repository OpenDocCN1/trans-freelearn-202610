Also, as Android users, the back action is really important. Each Android phone supports its own way of interacting with the back action – on some devices, it’s a dedicated hardware button; on others, it’s a software button with a reserved place (unless it’s in the context of a fullscreen app); or it can even be a gesture performed from the left edge of the screen. Regardless, consideration for what the back action should do in every context is important. Often, the default behavior will be good, but just as often you will want to customize it.

Good behavior typically means the back action matches user expectations, like navigating to the previous screen or exiting a modal. Bad behavior might involve unintentionally exiting an app, losing unsaved data, or breaking a user’s flow by sending them too far back in the navigation stack.

Customizing the back action could include:

+   - Preventing accidental exits by prompting users to confirm before leaving a critical flow (like discarding unsaved changes in a form).
+   - Redirecting users to a specific screen instead of the previous one, which can be helpful for guided workflows or when maintaining a consistent navigation structure.
+   - Triggering specific actions within an app, such as closing a menu or collapsing UI elements instead of exiting entirely.

Tailoring the back action ensures that users have a seamless and intuitive experience, especially when the default behavior doesn’t fit your app’s design or user flow.

Designing for diverse input methods – whether touch, keyboard, or alternative controllers – ensures accessibility for a wide range of users. However, not all interactions rely on physical input. Voice interfaces offer another powerful way for users to navigate and control applications, providing flexibility and accessibility for those who may have difficulty with traditional input methods. Let’s explore how to design voice interfaces that are both effective and inclusive.

# Designing voice interfaces for accessibility

Apple and Android tend to mirror each other’s operating system functionality. When a new feature appears in one, you can be sure that the other is aware and, not long later, it will often show up in its own way in the other. The same logic applies to the accessibility space.