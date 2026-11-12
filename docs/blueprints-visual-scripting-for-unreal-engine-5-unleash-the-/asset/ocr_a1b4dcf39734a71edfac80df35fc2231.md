Clicked event. This will add an event that triggers when the button is clicked:

![](bbox=[0.124, 0.153, 0.532, 0.364])

# Figure 8.37 – Adding a button event

+   13. You will be taken to the Graph view, where an On Clicked (Btnrestart) node will appear. Drag from the output pin of On Clicked (Btnrestart) onto Graph and add an Open Level (by Object Reference) node. In the Level parameter, select the level we are using, which is FirstPersonExampleMap. This node will reload the level when the player clicks on the button, resetting all aspects of the level, including targets, ammo collectibles, and the player.

+   14. Drag from the output pin of Open Level (by Object Reference) onto Graph and add a Remove from Parent node. This node removes the WinMenu widget from the view. We want the menu to go away once the level is reset:

![](bbox=[0.124, 0.694, 0.868, 0.821])

# Figure 8.38 – The actions of the Restart button