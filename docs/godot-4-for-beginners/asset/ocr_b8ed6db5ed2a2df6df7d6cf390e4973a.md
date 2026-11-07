💡 **Quick tip:** Enhance your coding experience with the **AI Code Explainer** and **Quick Copy** features. Open this book in the next-gen Packt Reader. Click the **Copy** button (1) to quickly copy code into your coding environment, or click the **Explain** button (2) to get the AI assistant to explain a block of code to you.

![](bbox=[0.14, 0.27, 0.21, 0.36])

```
function calculate(a, b) {
  return {sum: a + b};
};
```

🔒 **The next-gen Packt Reader** is included for free with the purchase of this book. Scan the QR code OR go to packtpub.com/unlock, then use the search bar to find this book by name. Double-check the edition shown to make sure you get the right one.

![](bbox=[0.22, 0.42, 0.38, 0.52])

Finally, we can make the scene more dynamic by making the label bounce around the screen like the famous DVD screensaver!

# Making the scene dynamic

To do this, we need to break the problem into smaller parts:

+   1. Give the label an initial velocity (speed and direction).
+   2. Find out the dimensions of the screen.
+   3. Find out the size of the label.
+   4. Test whether the label has reached the left or right edge and reverse direction.
+   5. Test whether the label has reached the top or bottom edge and reverse the direction.
+   6. Repeatedly update the label’s position so that it moves.