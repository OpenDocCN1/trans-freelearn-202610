
Finally, we'll need to make sure it's inexpensive. So, off to Amazon we go and this is what we arrived at:

![](img/504adb30bdfce87f38258e08f11180af_145_0.png)

It has enough motor power to allow for the variances in weight distribution, and gives us the ability to program the controller via USB!!! So far, our total budget is up to $496.94 US. OK we're doing good for a proof of concept (so far).

Now, how will we mount the Theta S to this gimbal? Well, enter our 3D printers from the previous chapters. We use our calliper measuring device (once the gimbal arrives) to measure the actual goPro mount, as well as the dimensions of our Theta S (at its widest point). The Theta S is kind of football-shaped on its y-axis (height-wise). This works well for us.

If we design a mount that fits a goPro's dimensions, but has a gap in the center that is square, a simple bead of silicone on the sides of where the Theta S will be pinched in will make it very strong and have enough friction to keep the Theta in place. Here's the simple (and lightweight) design we worked up:

![](img/504adb30bdfce87f38258e08f11180af_146_0.png)

There are a few design elements that should stand out to you:

- 1. The edges are bevelled so that if any measurement (or 3D printing) variance will make the adapter seat firmly into the mount.
- 2. There are pins and holes so that the adapter holds the Theta S without the danger of the two sides of the clamshell sliding apart.
- 3. The hole for the Theta S to slip into is square (and not football shaped).

Here is what the final 3D-printed adapter looks like:

![](img/504adb30bdfce87f38258e08f11180af_147_0.png)

It may be a bit difficult to see (as we also used transparent filament in our printer) the two beads of silicone sealant used on each half of the clamshell, but this is the reason it works so well. It offers a lot of traction, and is flexible enough to really put the squeeze on our Theta S. Also, notice that the 3D print is hollow with a honeycomb inside. This makes it nice and light, yet strong. Here's how it looks with the Theta S strapped in:

![](img/504adb30bdfce87f38258e08f11180af_147_1.png)

Seems immensely simple, right? It is. Sometimes the best solutions are. For proof of concept designs, sometimes rubber bands and zip ties are your best friends. You may be wondering if the rubber bands will interfere with the goPro mount. I assure you they won't. In fact, they'll come in handy. But we'll get there in the assembly stage.

So, our final weight is 146 grams, which is still less than a goPro. It's important to keep weighing things as you go with a multicopter. I apologize for the smudged scale. It's also the scale we use to weigh silicone ingredients before mixing. But trust me, it was zeroed (with the Theta neoprene cover already on it) before the weight; 146g is accurate.

![](img/504adb30bdfce87f38258e08f11180af_148_0.png)

OK, so far our total weight is 149 grams (gimbal) + 146 grams (camera) for a grand total of 295 grams. It's extremely important to keep track of weight while putting together an aerial drone (especially a multicopter). Now we just need a drone that can lift that payload and land safely.

## Landing gear

Well, we're in a pickle here. The problem is that we need retractable landing gear (as shown in the following screenshot):

![](img/504adb30bdfce87f38258e08f11180af_149_0.png)

Why? We're working with 360 VR video here. We want to just replace the drone (and put in more sky) not have to rotoscope out landing gear on the sides and below our camera. But the landing gear wouldn't arrive for a month! Additionally, there's no real way to mount our gear and our gimbal to the drone. We need a rail system.

Enter the landing gear as shown in the following screenshot:

![](img/504adb30bdfce87f38258e08f11180af_149_1.png)

Ugh. Well, they don't retract, but they do have rails so, we'll order both. We'll put the rails on the drone, and run our basic flight tests. Then, when the retracts arrive, we'll be all set to run some video tests. It'll cost us an extra $20.99, but who cares? In the grand scheme of things, it's a drop in the bucket. We're also in a time crunch. So if the retracts don't arrive in time, we can still run some tests and get this thing in the air!

What's the biggest thing these two have in common? Lots of clearance. That way, the extra length of our camera and gimbal will still clear flat ground/a landing pad. It's by far the most important part to keep the camera safe.

So far, our total budget has reached $561.63 US. And we haven't even bought our airframe, motors, propellers, and ESCs yet. Luckily, we already have a Pixhawk.

But we're not at that point. Now, since we really have no way of previewing our 360 video (without using Wi-Fi) on this camera... we'll need a second FPV (first person view) camera (so we can see what's happening with our drone from the ground station). We never want to use Wi-Fi on an aerial platform. There's too much possibility of interference with control signals. That would be full risk and you never want to go full risk.

## A first-person view – FPV system

In the US, it's illegal to fly a drone outside of visual line of sight (VLOS) without a special waiver. However, you're safest if when you look at a laptop screen, you can still see something about where your drone is headed, right? So an FPV system is a necessary safety feature (if used properly). Plus, it gives the director of your VR shoot an opportunity to see (first hand) where the drone is shooting from and direct you properly.

Since this is just a proof of concept, we don't need a very powerful system. It doesn't need a very long range, and as this is just a small version of a production drone it really needs to be as simple and light as possible.

Enter the (all in one unit) Spektrum FPV camera with an integrated transmitter:

![](img/504adb30bdfce87f38258e08f11180af_151_0.png)

That’s another 85 grams, bringing our total weight (so far) up to 295 grams + 200 grams (landing gear + rails) + 85 grams (camera/transmitter) = 580 grams. See how it just keeps getting bigger and bigger? But wait, there’s more!

This transmitter works at a much smaller voltage than our drone. We have two options: either put in some sort of transformer or just use another battery. We’ll go with the second option (for our proof of concept). Why? Because a 1S LiPo battery is probably going to be even lighter than a transformer:

![](img/504adb30bdfce87f38258e08f11180af_151_1.png)

The F550 with E300 propulsion system we bought are shown in the following screenshot:

![](img/504adb30bdfce87f38258e08f11180af_153_0.png)

The Flame Wheel airframe, motors, ESCs, and props weigh about 2.9 lbs (1,315.5 grams). That brings our total weight up to 1,897.1 grams (well below our maximum takeoff weight of 2,400 grams). This means we have about 502.9 g to spend on batteries, and Pixhawk.

## Batteries

We want 4S batteries. We need the power. Unfortunately, there's a weight problem in that the battery weight is increased by 25% over a 3S. (The s is how many cells the battery holds.)

We need to leave 150 g to 200 g for our Pixhawk + accessories (GPS, and so on). So we have 300 g to 350 g to spend on batteries. Unfortunately, that greatly limits our mAh (milliamp hour) rating. Basically, that rating is how many milliamps the battery can expend in one hour.

The bigger the number, the longer your potential flight time. Theoretically, in reality, the more mAh you have, the heavier your battery, and the more that affects your overall flight time. It's the law of diminishing returns. For example, you won't see twice the flight time from a 6,000 mAh battery as you would from a 3,000 mAh battery. It would be more like (not actual just for the purposes of an example) 15% (if you could even get the aircraft off the ground).

We found these batteries right in the sweet spot of 328 g:

![](img/504adb30bdfce87f38258e08f11180af_154_0.png)

- HRB Brand Lithium Polymer (LiPo) Battery
- 1 year warranty and friendly customer service.
- Application:RC airplane, helicopter, car, truck, boat, drone, etc.(Only if the voltage, dimension and the plug match, then it will fit).
- Notice: Don't over charge more than 4.2V and dont over discharge below 3.7V

That brings our monetary budget to $622.60 (so far) + $66.98 (two batteries) + $310 (Flame Wheel airframe) for a total of $1,000.58. Really, that's pretty inexpensive for a proof of concept drone.

Now, we need to run some calculations to see if this thing will even fly.
