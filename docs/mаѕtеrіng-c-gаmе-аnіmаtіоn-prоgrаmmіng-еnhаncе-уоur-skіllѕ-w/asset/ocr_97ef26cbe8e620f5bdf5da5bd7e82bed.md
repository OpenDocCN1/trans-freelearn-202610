# Enhancing Animation Controls

+   - Add animation blending for diagonal movement.
+   
+   Similarly to the previous task, instead of adding multiple directions, you can try to blend between the forward/backward and left/right animation clips when the instance is moving in a diagonal direction.

+   - Add a preview for the state order configuration.
+   
+   To preview the transition from Source to Destination state, the finite state machine would need to be adjusted with some sort of testing mode. The source state must be set and played for a while, i.e., until the animation clip is restarted. Then, the transition to the destination state must be triggered. Plus, we need to forbid the instance from moving; all the transitions must happen without changing the world position of the instance.

+   - Allow animation clips to be played in the reverse direction.
+   
+   The woman model has a clip with a sit-down animation. To make the instance stand up again, you could add a Boolean to play the clip either forward or in reverse direction.

+   - Add more speed interpolation functions and make them selectable.
+   
+   As mentioned in the Using acceleration and deceleration section, the interpolation between different speeds is done by a simple linear interpolation. Try to add more advanced interpolation variants here, like separate ease-in and ease-out functions based on curves. You could also add an extra field to the model settings that stores the interpolation function and make additional fields in the UI to choose an interpolation to use.

+   - Use a graphical tool to draw nodes and connections.
+   
+   The Graph Editor from ImGuizmo (see the link in the Addition resources section) adds nodes and connections to ImGui. You could change the mapping configurations to have nodes for states and animation clips and use the connections to define mappings.