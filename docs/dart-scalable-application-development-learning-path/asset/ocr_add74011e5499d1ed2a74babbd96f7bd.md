# Summary

Let me finish the story of Dart to JavaScript interoperation to highlight our expertise.

The core set of Dart libraries include `dart:js` to help you interoperate between the Dart and JavaScript code. The `dart:js` library converts the original JavaScript objects, functions, and collections with the help of `JsObject`, `JsFunction`, and `JsArray`. It supports automatic type of conversion in both directions. Dart supports a small subset of types, transferring directly from JavaScript types. All other JavaScript types are converted to Dart types with the help of a proxy. Dart Map and Iterable collections could be translated into JavaScript collections with the `jsify` constructor of `JsObject`.

We compared `jQuery`, `JProxy`, and `dart:js` and cleared the Dart code based on the `dart:html` solutions to identify who is quicker than the others. The `dart:html` library-based solution is the unbeatable champion and hero of this chapter.

In the next chapter, we will talk about how i18n and l10n accesses can be embedded into our code to help design and develop web applications that enable easy localization for different cultures, regions, and languages.