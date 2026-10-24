# Building the quadruped platform

You'll first need some parts, so you can build your quadruped robot. There are several kit possibilities out there, including at http://www.trossenrobotics.com/p/PhantomX-AX-12-Quadruped.aspx. However, such kits can be expensive; so for this example, you'll create your own kit using a set of Lynxmotion parts. These are available with several on-line retailers, such as http://www.robotshop.com/. To build this quadruped, you'll need two sets each of the two leg parts and then one set each of a body. Here are the parts with their Robotshop part number:

| Quantity | Description |
|---|---|
| 1 | Lynxmotion Symmetric Quadrapod Body Kit - Mini QBK-02 |
| 2 | Lynxmotion 3" Aluminum Femur Pair |
| 2 | Lynxmotion Robot Leg "A" Pair (No Servo) RL-01 |
| 4 | Lynxmotion Aluminum Multipurpose Servo Bracket Two Pack ASB-04 |
| 2 | Ball Bearing with Flange - 3mm ID (pair)
Product Code: RB-Lyn-317 |

The last part is not a Lynxmotion part but a bearing that you'll need to connect the legs to the body.

You'll also need 12 standard-size servos. There are several possible choices, but I personally like the Hitec servos. They are very inexpensive servos that you can get at most hobby shops and on-line electronics retailers. Now its time to get to know some details about the selection of the model of servo. Servos come in different model numbers, primarily based on the amount of torque they can generate.

**Torque** is the force that the servo can exert to move the part connected to it. In this case, your servos will need to lift and move the weight associated with your quadruped, so you'll need a servo with enough torque to do this. In this case, I suggest you use eight model HS-485HB servos. You'll use these for the servos attached to the end of the leg and for the body. Then you'll use four model HS-645MG servos for the middle of the leg; this is the servo that will require the highest amount of torque. You can also just use twelve HS-645MG servos, but they are more expensive than the HS-485, so using the two different servos will be less expensive.