
## Testing and driving

Before we take it out to the field, we'll want to test the connections. Close the Mission Planner software, remove the USB cable, and attach the USB telemetry dongle to your computer. With the wheels off the ground, power on your transmitter and rover by plugging the battery in, but do not arm the rover.

After starting the Mission Planner software, you may be asked again to find the rover's COM port. In your device manager, the telemetry unit should be visible under the Ports tab, this time as a Silicon Labs device.

Upon successfully finding the telemetry dongle, you'll see this screen inside Mission Planner:

![](img/504adb30bdfce87f38258e08f11180af_78_0.png)

After clicking on the Connect button (top right of the interface), you should see your rover at your location. Yep... that's my house.

Let's do some initial tuning and show you how it's done. Before we actually drive the rover, we need to make sure it's not just going to speed away at 70+ mph into oblivion.

At this point, with the telemetry module communicating with the computer, we can do anything we could do with USB. If you go to the Config Tuning tab at the top, it lets us set up some limits for the speed of our rover (as shown in the following screenshot):

![](img/504adb30bdfce87f38258e08f11180af_79_0.png)

Inside the Standard Params tab on the left, you'll find Target Cruise Speed and Base Throttle Percentage (as we saw earlier). We set our cruise speed at 5 meters per second and our throttle percentage at 15%. This way, on autopilot the throttle doesn't go above 15% and the speed doesn't exceed 5 meters per second—nice and slow.

Drive the rover around a lot before ever attempting an autopilot. We'll get deeper into tuning later. But for a rover, these two speed settings are essential. We can gradually increase them later if we wish.

### Setting waypoints

We're going to get much deeper into this in the next chapter. However, setting waypoints is as simple as clicking on the map on the main screen of Mission Planner. The only thing we really want to say at this point is that it risks everything. Your drone may crash, and could even hurt someone. We don't intend this chapter to show you how to create a waypoint drone. This one does not even have collision avoidance. However, this chapter was to familiarize you with the methodology of creating a rover-style drone, and the basic connections and configuration of Pixhawk 2.1.

This drone is waypoint capable though. If you must try it out, do it in a completely empty area where no people or property will get hurt.

For a full explanation of waypoints and autonomous driving, refer to the next chapter.

### Summary

In this chapter, we learned how to fabricate parts and connect up the older peripherals to the brand new Pixhawk 2.1. We got into the configuration wizard, and even touched on the telemetry module.

In the next chapter, we'll get deeper into all of these things with our first purpose-built drone, a duck decoy. This decoy will autonomously troll around a pond and attract ducks for hunters. We'll use everything we learned here and get into waypoints and patrolling with rovers.

So, buckle up... here we go!

## A Drone for Hunters – Autonomous Duck Decoy

A water rover is really no different than setting up a land rover drone; with one serious exception—water + electronics = bad... really bad. So, special attention needs to be paid to the waterproofing of your drone. In this regard, some special techniques, materials, and mechanics need to be employed to keep water out, keep your drone upright, and keep it alive while floating in an environment that completely destroys electronics.

In this chapter, we'll be wrapping up some of our manufacturing techniques, as well as getting a bit more into Pixhawk. But mostly, we'll show you how similar a boat is to a rover. We'll be covering:

- Coming up with ideas for drones
- Making your ideas marketable
- More on manufacturing and fabrication
- Foresight of design challenges
- The design workflow
- Waterproofing

Let's start with, "Why a duck decoy?" After all, doesn't that seem like a waste of Pixhawk? Isn't Pixhawk used for huge prototype medical drones? Isn't Pixhawk something meant for gazillion-dollar (to make and sell) ideas?

### Spawning a marketable deal

As any business major can tell you, the key to making money on a product is the law of supply and demand. The key here is demand. The best ideas—the ideas that changed the world and made the inventor a ton of capital—aren't necessarily the ideas that cater to a customer with an uber-budget. The wheel, the mouse trap, even the scrub-daddy are multimillion (even billion) dollar ideas that everyone can use, most can afford, and solve a basic problem.

Personally, I don't hunt, full disclosure. I've never been duck hunting. However, I have noticed that those I do know (who hunt) have duck decoys—lots of them. Different decoys for different types of ducks, and often they come back with fewer decoys than they left with.

Also, hunting is boring. Go out to an area and wait, and wait, and keep waiting until eventually some ducks show up. What if you could make the waiting process more fun? What if you could drive a duck around on the pond, or even let it drive itself and navigate the pond? What if you could show off a new toy to your buddies that they don't have? And most importantly, what if (when you're done for the day) you could have your duck return to you?

It solves some problems, it has the show-off factor, and it also caters to an industry that is spread throughout the country where accessories are essential and people spend a boatload (pun intended) on these accessories.

Best of all, even though you'd be using existing technology, the idea is potentially patentable! There are tons of ideas that use existing tech in new ways that have patents. So, not only can you make an item that is marketable... it's an idea that you could license and sit back and let money roll in. These are all great reasons to make a duck decoy drone.

As I said, I'm not a hunter. I love aircraft (especially the F-117 stealth fighter). So, knowing nothing about ducks (really), nor the hunting thereof, I made my duck decoy with a faceted design just to make the prototyping phase more fun. So meet the stealth duck (also known as Black Ops McDuck), as shown in the following image:

![](img/504adb30bdfce87f38258e08f11180af_83_0.png)

As you can see, it's 3D printed. But at least it's not pink, right?

### Outlining the scope before you design

Before we actually start putting pencil to paper (or rather mouse to CAD, in this case), we need to outline some basic needs and layout of the duck. Let's start with the problems and solutions analysis that we did on the design.

The following table lists the problems and solutions we worked up, although the original table was handwritten, this one is far more legible:

| Problems we'll encounter | Solutions |
|---|---|
| Water leaking into seams of the model | CA glue seams, and fill with silicone sealant. |
| Water leaking into the location where the propeller shaft meets up with the body | Stationary sleeve for shaft filled with grease and sealed (on the outside) with silicone. |
| Water leak on access lid | Silicone sealant o-ring. |
| In event of water leak, waterproofing Pixhawk | Silicone moulded Pixhawk case? (No). |
| Pixhawk heat from silicone moulded case? | Can't seal it. Must make body as water-tight as possible. Test water-tightness in every stage leading up to Pixhawk integration. |
| GPS reception with internal antennae | Shouldn't be a problem with a PLA plastic body. |
| Motor EM interference with GPS | Place motor up front and GPS antennae toward the back. |
| Motor heat in a sealed body | Water cooled motor. |
| ESC heat in a sealed body | Water cooled ESC, or low-voltage ESC / Motor. |
| Expense of motor, drive shaft, and prop | Kitbash a cheap RC boat. |
| Balancing motor, ESC, and prop for efficiency | Kitbash a cheap RC boat. |
| Duck is too large for 3D printer | Print in sections, use CA glue to join, and seal with silicone when dry. |
| Interfacing Pixhawk with cheap RC boat parts | Make sure the boat we buy is modular (not integrated ESC or servo into receiver) or at least minimize integrated parts so we have to replace as few parts as possible. |
| What if we want the head to turn with rudder? | Don't do this on initial prototype. But print head as a separate section so it can be adapted later if we want. |
| PLA plastic print or ABS? | PLA for the body (cheaper). But ABS for motor mount (higher melting temperature). |
| Tipping from waves or turns? | Large ballast section filled with modeling clay. |

We had other problems that we encountered along the way, which were unforeseen. We'll get to these later. But as you can see from the details in our overview chart, we tried to minimize these as much as possible. Positive thinking is great. But when planning, you have to be your own nay-sayer in order to predict the problems you'll have. Then, you can think positively to come up with solutions.
