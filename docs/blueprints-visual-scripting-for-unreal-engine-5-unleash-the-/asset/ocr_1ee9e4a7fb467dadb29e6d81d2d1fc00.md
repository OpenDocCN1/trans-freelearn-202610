# # Changing the Actor's Mobility and Collision settings

To allow our target to move, we first must change its Mobility setting to Moveable. This allows an Actor to be manipulated while playing the game. To do this, follow these steps:

# ## Important Information

By default, basic Actors that are placed in the world are set to Static. Static means that an object cannot move or be manipulated during gameplay. Static objects are significantly less resource-intensive to render, and this should be our default choice for non-interactive objects so that we can maximize frame rates.

+   1. Open BP_CylinderTarget again. On the Components panel, select Static Mesh Component. On the Details panel, switch the Mobility toggle, located underneath the TRANSFORM properties, from Static to Movable:

![](bbox=[0.125, 0.55, 0.866, 0.815])

Figure 5.22 – Changing Mobility to Movable