Variables are used to represent the current state of a Blueprint, but the behavior is defined by events and actions, which will be discussed in the following section.

# Defining the behavior of a Blueprint with events and actions

Most of the time, we will use Blueprints to create new Actors. In Unreal Engine, Actors are game objects that can be added to a level.

Unreal Engine informs the state of a game for an Actor using events. We define how an Actor responds to an event by using actions. Both events and actions are represented by nodes in the **Event Graph** panel.

# Events

To add events to a Blueprint, use the **Event Graph** panel. Right-click the **Event Graph** panel to open **Context Menu**, which has a list of available events and actions. If you need more space in the **Event Graph** panel, you can right-click and drag to move it to an empty area of **Event Graph**. **Context Menu** has a **Search** bar that can be used to filter the list of nodes. There is also the **Context Sensitive** checkbox that filters the possible actions based on the node selected. The following screenshot shows **Context Menu** and some of the events available: