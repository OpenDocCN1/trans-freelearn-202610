In the preceding diagram, we have a ball and a cylinder that have been combined into one object. Notice that there is no cylinder geometry inside the ball. It's one solid object. Whereas, in the following diagram, we have that same ball and cylinder, but the cylinder has been used to cut a hole in the ball. This is done by creating two objects and using the Boolean function:

![](bbox=[0.1, 0.27, 0.9, 0.6])

This method is how we created not just an access hole, but the lid on our duck. We simply created a basic shape, and used it once to cut (Boolean subtract) a hole. Then, we used that shape and the original duck again to create a lit (using Boolean intersect). Here, you can see them shown in different colours (as shown in the following diagram):