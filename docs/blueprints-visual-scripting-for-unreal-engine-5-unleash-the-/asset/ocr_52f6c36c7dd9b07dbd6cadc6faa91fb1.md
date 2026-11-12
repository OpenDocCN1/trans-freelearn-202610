![](bbox=[0.126, 0.092, 0.868, 0.515])

Figure 16.6 – Interacting with the Widget menu

In the EventGraph of **VRPawn**, there are several input events from motion controllers, which we will see in detail in the _Teleportation_ and _Object grabbing_ sections.

# # Teleportation

In this section, we’ll see the event and functions used in teleportation.

To start the teleport, press up on the right motion controller’s thumbstick to mark a location. When you release the thumbstick, you will teleport to the marked location. The teleport destination is represented by the **VRTeleportVisualizer** Blueprint: