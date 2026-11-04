# Important note

You may have noticed the similarity between the external fluid volume we can create with direct modeling and the simplify command we covered in the previous subsection related to parametric modeling. The underlying functionality between these two features is the same. However, as previously discussed, the SIMPLIFY feature is not entirely parametric and therefore could create issues when used in parametric modeling.

The next function, unique to direct modeling when working with imported CAD geometry, is one of my favorite features in Fusion 360. It is called FIND FEATURES and is in the SOLID tab | the CREATE panel. It allows you to select faces, bodies, or components and automatically recognize critical features, such as fillets, holes, chamfers, and patterns. After you use the find features command, and recognize a critical feature such as a fillet, it will show up in Fusion 360’s browser with its designated name. You will be able to select the feature, right-click, and edit it. Depending on the position of the feature, you can easily change the dimensions of a given feature or completely delete it. We will cover how to use this command more effectively in the next section.

Direct modeling, as its name suggests, is not a parametric method for creating and editing geometry. Many of the modeling tools, such as fillet, hole, and pattern, have different behaviors during creation versus editing. Any feature you add to a solid model utilizes the exact same user interface as its counterpart from parametric modeling. The Fillet feature is a great example of this. In Figure 2.21, we are showing the application of a fillet to multiple edges.

![](bbox=[0.13, 0.51, 0.87, 0.82])

Figure 2.21 – Creating the Fillet feature in Direct modeling