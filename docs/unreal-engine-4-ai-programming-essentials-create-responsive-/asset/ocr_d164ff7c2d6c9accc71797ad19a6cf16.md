# # What Have We Learned?

In this chapter, we will briefly glance at some of the chapters. This chapter is important because we will discover the caveats of what we've done. Lastly, we will talk about other examples of what we could achieve with these combined lessons.

In each chapter, the goal was to start with an objective. Then, we approached the objective with a practical demonstration of the knowledge. We talked about what AI means to game AI. We then demonstrated the basics of creating game AI within Unreal Engine 4. What we didn't cover are the issues we may run into. What were the weaknesses and benefits of the techniques we demonstrated? In this chapter, we will answer these questions.

# ## Creating basic AI

There are other forms of AI available. In this book, we created the basic AI. In the hierarchy of the components, there is a controller. The controller then decides which Behavior Tree to run. Then, tasks are chosen based on the sequence of decisions in the tree.

In the controller, we could have an array of different Behavior Trees suited for different tasks. Then, once we approach the task, the tree contains the subtask which aids in coming to a solution. Introducing this level of abstraction requires understanding of which state is necessary to enter.