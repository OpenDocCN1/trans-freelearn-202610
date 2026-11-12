+   7. Drag a wire from the top input pin of the **Equal** node and add a **GET Patrol Point 1** node.

+   8. Connect the bottom input pin of the **Equal** node to the **Other Actor** output pin of the **Event ActorBeginOverlap** node.

+   9. The nodes of _steps 9–11_ set **Patrol Point 2** as **Current Patrol Point**. Drag a wire from the **True** output pin of the **Branch** node and add a **SET Current Patrol Point** node.

+   10. Drag a wire from the input pin of the **SET Current Patrol Point** node and add a **GET Patrol Point 2** node.

+   11. Drag a wire from the white output pin of the **SET Current Patrol Point** node and add the **UpdatePatrolPointBB** macro node.

+   12. The nodes of _steps 12–15_ are to check whether the enemy overlaps with **Patrol Point 2**. Drag a wire from the **False** output pin of the **Branch** node and add another **Branch** node.

+   13. Drag a wire from the **Condition** input pin of the second **Branch** node and add an **Equal** node.

+   14. Connect the top input pin of the **Equal** node to the **Other Actor** output pin of the **Event ActorBeginOverlap** node.

+   15. Drag a wire from the bottom input pin of the **Equal** node and add a **GET Patrol Point 2** node.

+   16. The nodes of _steps 16–18_ set **Patrol Point 1** as **Current Patrol Point**. Drag a wire from the **True** output pin of the second **Branch** node and add a **SET Current Patrol Point** node.

+   17. Drag a wire from the input pin of the new **SET Current Patrol Point** node and add a **GET Patrol Point 1** node.

+   18. Connect the white output pin of the **SET Current Patrol Point** node to the input pin of the **UpdatePatrolPointBB** node.

+   19. Compile and save the Blueprint.

These are the actions needed in the `BP_EnemyCharacter` Blueprint to handle the patrol points. The next step is to modify the `BP_EnemyController` Blueprint to run the Behavior Tree.