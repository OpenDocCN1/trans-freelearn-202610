
The industry stopped resisting the term drone. I remember on my first drone book with Packt, *Building Multicopter Video Drones*, resisting the word *drone* in the title. But they convinced me it would be good for search engine food. As we (in the industry) stopped resisting the term and started educating the public about the safety, best practices, and usefulness of drones the negative implications lost all meaning. And (as in the following image), drones even took on an entirely new meaning:

![](img/504adb30bdfce87f38258e08f11180af_23_0.png)

A group of drone pilots from "Humanitarian Drones" and FEMA team members assisting in Port Arthur, TX during Hurricane Harvey relief efforts. Photo by Daniel Herbert (of Humanitarian Drones)

Suddenly, drones were saving lives (with search and rescue), helping people recover from disasters (with assessment of damage, as shown in the following image), and responsible for great shots in feature films and people’s favourite documentaries. Then, something completely weird happened:

![](img/504adb30bdfce87f38258e08f11180af_24_0.png)

A DJI Inspire 2 conducting damage assessment and mapping in the aftermath of Hurricane Harvey in Houston, Texas. Photo taken by Brian Scott of Humanitarian Drones

Drones were no longer multicopters. The term covered anything that flew under remote control. This happened with the FAA's small Unmanned Aerial System (sUAS) rules. The media dubbed these rules drone legislation. Suddenly, drones had wings, even jet engines. Now, they didn't even have to fly autonomously, the radio-controlled airplane that grandpa flew on weekends was a drone.

Then, it went further. The guidance systems used in multicopter drones were being repurposed to control ground vehicles (rovers) and water vehicles (boats and submersibles). Now, in 2017, you actually hear companies coming out with underwater drones.

In short, the answer to the original question, "What (exactly) is a drone?" is simply a robot-a vehicle of any type that is either not controlled by a human (autonomous) or controlled by a human via remote control. It may not be the Oxford English Dictionary's definition, but it is the definition that the common public accepts when they hear the word drone.

And, as for the proof that we're living in the Drone Era? In the height of the space age (the mid-1980s), men on rocket packs flew into stadiums for the opening ceremonies for the 1984 Olympics, and even during the Super Bowl XIX half-time show. Now, we have drone-shows for those same venues. Of course, we still have a ways to go for full-fit drone acceptance. After all, the drone part of the half-time show had to be filmed days earlier for fear of a drone attack during the show, or at least fear of mass hysteria in the venue. But we'll take it.

## Purposes for drones

Drones are just for taking pictures, right? Wrong! Drones can be useful for just about anything and everything. Here are just a few examples:

- Suffering in a drought (hello, California) but need to water your crops? You can use a drone to map your whole farm. Then, run analysis on crop health, so that you know where to direct water. All the while sipping iced tea on your porch.
- Want to walk the golf course, but don't want to hire a caddy or carry your own clubs? Caddy drone to the rescue! Make a motorized golf trolley, and have it follow the GPS signal on your cell phone as your walk the golf course.
- Maybe you're a herpetologist on an expedition to a remote location and you found a new species of tree frog. Get it back to base-camp from miles away in an autonomous powered glider. Just load the frog into the payload bay, give it a toss above the treeline and let it fly (and land at) home! Then, just wait for base camp to get the specimen and send the powered glider back to you for your expedition to continue.
- You're a lifeguard at a beach and see someone having trouble. Launch an aerial drone that drops a boat-drone to tow the person back to shore. All the while, you don't have to leave your station (leaving the rest of the beach-goers without a lifeguard). You can meet the victim at the beach with medical supplies you may need and you're not too exhausted to help out!

The list goes on and on and on. If you can think of something requiring delivery of an item, manual labour requiring movement to another location, or just something to save time, a purpose for a drone will be found. The possibilities are quite literally limitless.

These are certainly some altruistic and noble uses for swarming, but the military also sees the benefit of drone swarms. The following image shows a recent test of drone swarms (deployed by dropping them from a pair of f-18 Hornets) used to map out a battle-field in real-time for mission commanders and soldiers on the ground:

![](img/504adb30bdfce87f38258e08f11180af_27_0.png)

Drone swarms can also be used by the entertainment industry to cover sporting events, if one drone is running low on batteries, another takes off to take its place. Drone swarms could be deployed over golf courses to help enthusiasts find their ill-hit balls that have gone off into the weeds. Farmers could use drone swarms to monitor crops, plant seeds, monitor watering patterns (and waste from evaporating water, or atomized sprinklers going into the air).

Again, the uses for drones are limitless.

We could list potential uses for drones all day, all night, every day, and every year, for our lifetimes. So, how do we realize this potential?

Well, to figure out how to make drones, we'll always first start off with how to implement existing tech and components to make what we want. The first step is to think in terms of what types of drones exist currently.

## Types of drones

Drones come in two categories with several subcategories. There are surface/subsurface drones, and aerial drones. Let's dive further into each.

### USV drones

USV simply stands for either Unmanned Surface Vehicle or Unmanned Submersible Vehicle. The same acronym of USV is used for both. These types of drones are usually referred to as Rovers or Submersibles respectively.

### Rovers

Rovers usually either have treads (like a tank) or wheels (like a car). The Mars Rovers are great examples of USV Rovers. They move along the surface of Mars taking various readings and measurements in an effort to explore the surface of another planet without the risk to human life. The following picture shows one of these Mars Rovers:

![](img/504adb30bdfce87f38258e08f11180af_28_0.png)

Not every endeavor for a rover needs to be quite so lofty as to explore another planet. Rovers are currently used in aspects of our daily lives (the iRobot Roomba is a daily life example of a USV Rover) all the way through exploring our own planet (rovers designed to explore lava flows—areas far too dangerous and inhospitable to humans).

However, rovers don't have to simply be driven across the ground. Some rovers are actually boats designed to skid across the surface of water for various purposes. An example of boat-drone USV is the one designed by the US Navy to escort ships through pirate-ridden waters, and protect naval vessels from other hostile vessels in dangerous waters (eliminating, or at least reducing, the need for bloated battle-groups).

In this book, we will be designing three surface rovers:

- A basic autonomous RC car which serves no purpose other than your education on the basics of implementing the Ardupilot interface.
- A water USV that serves the purpose of a duck decoy which autonomously navigates pre-planned routes around a pond for hunters. Hmm... one wonders what that might go for in a store?
- A ground USV golf caddy. This will show you how to make a rover follow a GPS tracker (in the form of the user's cell phone). This caddy is designed to move your clubs around the golf course without having to carry your bag, nor even tell the caddy where to go. It will simply follow you.

### Submersibles

Submersibles are vehicles designed to go beneath the surface of a liquid. Traditionally, this liquid is water. However, submersible vehicles could potentially be used to monitor other liquids, such as fermentation tanks at breweries, mixing tanks at chemical plants, and diving into raw sewage tanks. Other than diving into a vat of beer, I'm sure you can see how using drones is preferable to diving in yourself.

We won't be attempting to create a submersible in this book. However, the techniques employed are very similar to the subject matter of this book. Submersibles should only be attempted by highly experienced drone designers, as they'll have to contend with reduced ability to utilize GPS, reduced radio transmission distance, and highly reduced visibility in water (or other liquids). Therefore, technologies such as **forward looking infrared (FLIR)**, signal repeaters, and collision avoidance (via SONAR) should be considered. However, as you can see from the following image, the similarities between underwater drones and fixed-wing aerial drones (airplanes) are fairly obvious:
