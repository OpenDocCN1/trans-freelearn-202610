# How it works...

In step 1, you could read the is="dom-extend" attribute meaning, this is <div>, but it inherits the style, structure, and behavior of the dom-extend Polymer component. Step 2 shows us that class Domextend extends DivElement with Polymer, observable that Domextend itself inherits from class DivElement and mixes with the Polymer and Observable classes. Compare this with what we have for a normal Polymer component (from the Binding and repeating over a list recipe in this chapter), which only inherits from the PolymerElement class; class Pollist extends PolymerElement.

The polymer class is the mixing class for Polymer elements; it provides utility features on top of the custom web elements standard. If it is used in this way, you must call polymerCreated() from within the constructor. The constructor also changes the text in the <div> tag.

In step 3, we have a second but necessary statement, <polymer-element name="dom-extend"extends="div"> with extends="div", which Domextend inherits from DivElement in full.

We have also used the host selector in order to style the shadow DOM <style>:host { ... } </style>. This style can be overridden by the embedding web page. If this is not what you want, just use the normal style selectors, such as class, ID, and so on. The <content></content> area is used for content insertion; when a component has children, those children go where the <content> tags are. Here, the initial text is overwritten by the Polymer component dom-extend.

# Working with custom elements

Instead of making a new Polymer component from scratch or starting with an existing HTML element and building upon that as we did in the previous recipe, you can also simply use custom-made Polymer elements.

This recipe will implement some of the core and paper elements of the Polymer project (http://www.polymer-project.org/docs/elements/). The paper_elements project is the Polymer implementation of Google's Material Design UI widgets (for more information, refer to http://www.polymer-project.org/docs/elements/material.html). There will be more and more of these, either written in JavaScript with a custom Dart wrapper to use them, or purely in Dart, and you can also combine them with your own Polymer components. You can find the code in the project pol_custom.