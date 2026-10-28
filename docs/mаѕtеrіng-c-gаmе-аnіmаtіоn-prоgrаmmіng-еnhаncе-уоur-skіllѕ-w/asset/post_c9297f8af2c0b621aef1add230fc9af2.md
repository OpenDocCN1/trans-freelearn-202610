
## Summary

In this chapter, we improved animation processing efficiency and added gameplay-like controls to map animation clips to the current animation state. We started by moving the computational work of animation blending from the CPU to the GPU and created lookup tables to reduce the amount of work for the GPU (at the cost of memory usage). Then, we added the different movement states to the code, including a UI-based mapping between states and animation clips. As the last step, we added the new mappings to the YAML configuration file, allowing us to save and restore the mappings.

In the next chapter, we will take a closer look at collision detection. After we can move the instance in the virtual world, we need to avoid just *running through* other instances on the screen. As the first step, we will explore the theoretical background of collision detection and discuss the problems of a naive implementation. Then, we will add spatial partitioning to the virtual world and instance simplifications to reduce the complexity of the collision detection checks. Finally, we will implement multi-level collision detection, allowing us to detect collisions between instances with minimal costs.

## Practical sessions

There are some additions you could make to the code:

- Use root motion to control instance movement.

Instead of moving the position of the instances around the virtual world and playing the animation at a specific rate, so-called **Root Motion** can be used to let the animation clips control the movement of the character. In root motion, the movement of the model’s root bone is coordinated by the animation and not by us, allowing a better sync of the feet on the ground. However, root motion data must be baked into model animations to work. Updating the animations is beyond the scope of the book.

- Add a second direction for the idle/walk/run mapping.

Right now, only animations for the four main directions (forward, backward, left, and right) plus the special wildcards none and any can be configured. Moving diagonally may look a bit awkward when the feet are not synchronized with the instance movement.

## Enhancing Animation Controls

- Add animation blending for diagonal movement.

Similarly to the previous task, instead of adding multiple directions, you can try to blend between the forward/backward and left/right animation clips when the instance is moving in a diagonal direction.

- Add a preview for the state order configuration.

To preview the transition from Source to Destination state, the finite state machine would need to be adjusted with some sort of testing mode. The source state must be set and played for a while, i.e., until the animation clip is restarted. Then, the transition to the destination state must be triggered. Plus, we need to forbid the instance from moving; all the transitions must happen without changing the world position of the instance.

- Allow animation clips to be played in the reverse direction.

The woman model has a clip with a sit-down animation. To make the instance stand up again, you could add a Boolean to play the clip either forward or in reverse direction.

- Add more speed interpolation functions and make them selectable.

As mentioned in the Using acceleration and deceleration section, the interpolation between different speeds is done by a simple linear interpolation. Try to add more advanced interpolation variants here, like separate ease-in and ease-out functions based on curves. You could also add an extra field to the model settings that stores the interpolation function and make additional fields in the UI to choose an interpolation to use.

- Use a graphical tool to draw nodes and connections.

The Graph Editor from ImGuizmo (see the link in the Addition resources section) adds nodes and connections to ImGui. You could change the mapping configurations to have nodes for states and animation clips and use the connections to define mappings.

## Additional resources

ImGuizmo GitHub repo: https://github.com/CedricGuillemet/ImGuizmo

## Join our community on Discord

Join our community's Discord space for discussions with the author and other readers: https://packt.link/cppgameanimation

![](img/3cfb9832c943f5f7366fd104b5255971_253_0.png)

## Technical requirements

The example code is in the chapter08 folder, in the subfolders 01_opengl_collisions for OpenGL and 02_vulkan_collisions for Vulkan.

## The complexities of collision detection

We already talked about the complexity of finding a collision in Chapter 3 when deciding how to implement a visual selection, either using ray shooting or buffer drawing. We’ve chosen to draw the instances into a separate buffer, avoiding collision detection entirely.

Now is the right time to do a short reprise of the complex topic and to present solutions to accelerate finding collisions between instances.

## Avoiding the naive way

If we would check every triangle of every instance against all triangles of all other instances for collisions in the virtual world, this would come with immense processing costs. These simple, brute-force collision checks would grow exponentially, making it impossible to keep up a reasonable frame time when adding more and more instances.

Instead of using the naive solution, we should take a step back and think about possible types of simplification before implementing any kind of collision detection.

One idea is to reduce the number of instances we have to check against. Why bother with instances in distant parts of the virtual world? Even if our object is some kind of bullet, rocket, or other flying entity, all we can possibly hit needs to be “less than an arm’s length” away from us. All other objects can be safely ignored.

To achieve this reduction, we can split the world into different areas. And suddenly, we only have to check the area we are in. Maybe we have to check adjacent areas too, depending on the algorithm we use, but the total number of instances we have to check against can be reduced by a large amount.

On the other hand, we can lower the computational work for collision detection by reducing the amount of surface elements to test if we are even close to any of the triangles of the instance. Any kind of simplification will become handy; the fewer intersections to test, the better.

Representing an instance as a box or sphere may produce a lot of false results, but if the collision check against the box or sphere around the instance already fails, we can immediately sort out this instance from our list of possible collision targets.

Both ideas combined – reducing the number of instances to check and lowering the complexity of the instance checks – help to do collision checks in real time, even with larger numbers of detailed models.

Let’s start with the spatial partitioning of the virtual world.

## Using spatial partitioning to reduce complexity

In this section, we will take a look at some methods to split our world space into different parts, lowering the number of instances in each of the parts. We start with the simplest variant of spatial partitioning in two or three dimensions, the grid.

## Grid

In a grid, the virtual lines divide the virtual world into equally sized squares or rectangles, or equally sized cubes and cuboids.

![](img/3cfb9832c943f5f7366fd104b5255971_257_0.png)

Figure 8.1: 2D and 3D grids

While grids are easy to create, Figure 8.1 already shows some of the problems. Objects larger than the grid spacing must be placed into all overlapping grid fields, requiring checking all affected fields for other instances.

And while the vast majority of the grid will remain empty, “crowded places” inside the virtual world can lead to many instances inside a single field of the grid. Many instances mean many checks, and an uneven distribution of instances may cause slowdowns due to the number of calculations.

A sort of successor to the grid is the quadtree. A quadtree addresses the uneven distribution problem of the grid.
