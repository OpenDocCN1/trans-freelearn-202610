![](bbox=[0.13, 0.09, 0.86, 0.42])

Figure 2.23 – Making a duplicated component independent from its parent

By breaking its link from the source component and making it independent, any changes we make to the original component will not propagate down to the duplicate. Similarly, any changes we make to the duplicated component will not impact the original component.

In parametric modeling, if you want to achieve similar results, you can only utilize copy component paste new component option. Or you can use the Break Link option after inserting a component into a design, as shown in Figure 2.24. There is no method for making a component independent after duplicating it using one of the three patterning methods located within the SOLID tab | the CREATE panel | the PATTERN pull-out menu (Rectangular Pattern, Pattern on a path, or Circular Pattern):