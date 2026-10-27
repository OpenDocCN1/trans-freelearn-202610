# Checking the items of a collection on a condition

Sometimes, you want to know if all or any of the objects in a collection comply with a specific condition. The following methods check whether the collection conforms to specific conditions:

+   - **every**: This method returns `true` if every element in the collection satisfies the specified condition. This can be seen in the following code:
+   ```
+   List colors = ['red', 'green', 'blue'];
+   print(colors.every((color) => color != null || color != ''));
+   // => true
+   ```
+   - **any**: This method returns `true` if one element in the collection satisfies the specified condition. This is illustrated in the following code:
+   ```
+   List colors = ['red', 'green', 'blue'];
+   print(colors.any((color) => color == 'red'));
+   // => true
+   ```

# The iterate over collection

The following method helps you iterate through elements of the collection:

+   - **forEach**: This method applies the specified function to each element of the collection. This can be seen in the following code:
+   ```
+   List colors = ['red', 'green', 'blue'];
+   colors.forEach((color) => print(color));
+   // => red
+   // => green
+   // => blue
+   ```

# The search over collection

The search over collection list includes the following methods that are used to search for an element in a collection:

+   - **contains**: This method returns `true` if the collection contains an element that is equal to the requested one. This is shown in the following code:
+   ```
+   List colors = ['red', 'green', 'blue'];
+   print(colors.contains('red'));
+   // => true
+   ```