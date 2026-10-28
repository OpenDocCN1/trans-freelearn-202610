# # Animating game characters – a primer

Moving a game character around in a virtual world with lots of different animations, changeable outfits, a collision detection system for other characters and the environment, and maybe even interaction with other characters looks nice and simple when playing a game.

But the mathematics and techniques behind the smoothly animated game characters are extensive and complex. Every movement, animation, action, or state change of the character involves a long journey until the final image is rendered to the screen.

Let’s look at a high-level explanation of animations first. If you already know the details, you can skip to the next section.

# # About nodes, bones, skeletal animation, and skinning

The building blocks of an animated three-dimensional character model are the so-called nodes. A node can be compared to a joint in the virtual body of the character, like the shoulder or hip.

All nodes in the character model are connected in the form of a virtual skeleton, forming the bones of the model. By attaching child nodes to a node, modeling an arm with a hand and fingers, or a leg with a foot and toes – or even an entire human-like skeleton – is no problem. The starting point of the virtual skeleton is the so-called root node. The root node has no parent node and is used as the starting point for animations.

Usually, the level of detail does not reach the details of a skeleton in a real-world object since many of the real bones are static or play only a minor role in muscle or pose changes during animation.

The virtual skeleton of the character model can be animated by rotating nodes around their center point – and thus rotating the bone to all attached child nodes around the center point of this node. Just imagine raising your arm a bit: your upper arm will rotate around your shoulder joint, and the lower arm, hand, and finger follow the shoulder rotation. This kind of animation is called a skeletal animation.

A character needs to be stored in a more or less natural pose in the file, which is called the reference pose, or bind pose. You will find most models in a T-pose where both arms create a horizontal line, and sometimes you see the A-pose, where the position of the arms of the skeleton resembles the uppercase letter A.