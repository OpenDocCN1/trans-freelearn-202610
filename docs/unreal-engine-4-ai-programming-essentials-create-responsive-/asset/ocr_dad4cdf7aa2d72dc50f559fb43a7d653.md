+   - Second, we want to make the AI character follow some basic path. For example, we'll have the AI move along the walls in one direction. We can take our existing project and modify the AIController with new instructions. From there, our AI will now move along the walls in one direction indefinitely.
+   - Third, we want to make a new enemy AI character that will chase the first AI character we made—that is, Hero.

> > We will have to make additional changes to Hero to give it the ability to run directly away from Enemy.

Enemy will simply be instructed to move toward Hero every second.

# # Setting up the project

Let's open up Unreal Engine 4! We will begin with the first process of creating a new project.

> > We will use Unreal Engine 4.6.0 throughout this book. The instructions may vary per version. We will present the idea behind our actions as we demonstrate them using Unreal Engine 4; so, hopefully, you will be able to translate the instructions as you see fit.