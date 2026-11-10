
## Projects
### SHOWCASE
### FREDERICK VANDERBOSCH

Frederick is an electronics hobbyist and software test and integration engineer. One of his recent projects won Element14’s ‘Forget Me Not’ challenge.
frederickvandenbosch.be

![](img/d8322379620245fcd21eccc3855b8c78_73_0.png)

A Raspberry Pi-powered smart display rises out of the desk

An LED light strip is programmed to change colour and send alerts to the desk user

Touch controls are hidden in the bottom-right of the table. Tapping them toggles desk features on and off

![](img/d8322379620245fcd21eccc3855b8c78_73_1.png)

## PIDESK

Make any desk job exciting with this futuristic PiDesk: a touch-controlled table with lights, sound, and a computer that rises from the surface

## Quick Facts

- The entire build took about 18 weeks
- Frederick divided the project into sub-tasks to stay motivated
- The monitor lift is a drawer mechanism turned sideways
- The time it takes the screen to rise matches the Pi's boot time
- A USB sound card is used to power the built-in speakers

Nobody wants a boring desk, so one hobbyist, Frederick Vanderbosch, decided to build this futuristic workstation. Complete with a touch surface, speakers, and a motorised display that rises out of the table, the PiDesk is one of the cleverest projects we’ve come across.

“The build was part of a design challenge,” says Frederick. The Sci-Fi Your Pi competition was launched by the Raspberry Pi Foundation and Element14 to inspire inventors to build smarter homes.

“PiDesk is an attempt at making a space-saving, futuristic-looking desk,” Frederick explains. “It can change from a regular desk to a computer workstation and back at the touch of a finger.”

The idea for the project came when he was running out of space in his workplace. “By combining a computer workstation and desk in one, I would be able to get extra space to work when the computer was hidden inside the desk. For the project’s futuristic accents, I was inspired by the Tron movies, on which I based the light patterns of the desk.”

At first glance, it looks like a normal desk. As soon as the user places their hand above a specific area, however, a desktop computer is powered on and starts rising out of the desk. This action is accompanied by visual and audio effects.

“Two Raspberry Pis are involved in the project,” says Frederick. One serves as the brains of the desk; the second is a built-in desktop computer. As well as two Raspberry Pi units, the table contains a broad range of components.

The desk itself is from Ikea.

“It’s one of the cheaper models, which have the advantage of being hollow,” reveals Frederick.

“I started by drawing some shapes on the desk to work out where I would perform the cuts and embed the electronics.

"Then the surface was recreated using two layers. The first layer was paper, which is used to diffuse the light and hide all the cuts and embedded electronics. The second layer is a large sheet of transparent Plexiglass, giving the desk a new and shiny surface."

![](img/d8322379620245fcd21eccc3855b8c78_74_0.png)

![](img/d8322379620245fcd21eccc3855b8c78_74_1.png)

Touch controls are embedded into the surface using a combination of conductive paint and copper tape. The paint creates touch-sensitive pads, while the tape makes a connection to the Raspberry Pi.

One neat feature you might miss is the built-in wireless charger. This is a "little bonus feature I had in mind," says Frederick. The Magic Lamp turns on when it is placed in the right spot. "The wireless charger is embedded in the desk, and when the lamp is moved on top of it, the lamp is powered." The light consists of the wireless charger receiver, a microcontroller board, and a ring of Adafruit NeoPixels.

If you’re thinking of building a similar project, then planning is vital. "Plan ahead and think things through," advises Frederick. "Break down the project into smaller, more manageable projects. There’s a lot to learn by building a project like this because so many different components are involved."

Above left The built-in LED strip and Magic Lamp are especially effective in low light

Above The finished product is a usable desk with a display that can be lowered to provide more surface area

## BUILDING A PIDESK

![](img/d8322379620245fcd21eccc3855b8c78_74_2.png)

![](img/d8322379620245fcd21eccc3855b8c78_74_3.png)

![](img/d8322379620245fcd21eccc3855b8c78_74_4.png)

## >STEP-01
### Double Pi

Inside the PiDesk are two Raspberry Pis. One controls the desk interaction (lights, touch interface, and motorised display); the second acts as a built-in computer for the desk.

## >STEP-02
### Lights and controls

The desk itself is a cheap model with a hollow centre. Holes are cut into the surface and the components are placed inside. The cuts are filled in with paper and Plexiglass to recreate a flat surface.

## >STEP-03
### Desk assembly

A WS2812 LED strip is embedded into the desk to act as information lighting. Meanwhile, touch-sensitive pads are embedded to create interactive controls. A wireless charger is placed in the top-left to power the Magic Lamp.

## THE GREAT RASPBERRY PI SPOOK-OFF

Get a look at these Raspberry Pi Halloween projects that are absolutely to die for...

Gather round, ghouls and girls, it’s that time of the year again when scary rules supreme. Perhaps you’re planning on dressing up to go trick-or-treating or marathoning a ghastly amount of horror films, but we’ve found some people who are deep in their lairs experimenting with a Raspberry Pi to create the spookiest projects the world has ever seen. We’ve hunted down the most horrifying and wicked projects for your reading pleasure, but don’t worry, the only dark art at work here is the odd bit of C programming. Beware, read any further and you’ll be doomed to be inspired by these seven unholy projects... and have to read many more awful puns.

|   |   |
|---|---|
| 5 | ABS-GHOUL-UTELY TERRIFYING |
| 4 | SPECTRE-CULARLY FRIGHTENING |
| 3 | DEADLY SCARY |
| 2 | GRAVELY CREEPY |
| 1 | QUITE SPOOKY |

![](img/d8322379620245fcd21eccc3855b8c78_75_0.png)

## RASPBERRY PI HAUNTED HOUSE

Enter if you dare to the abode with home scare-tomation

**STEWART WATKISS**
Data centre manager, father,
part-time Count Dracula
magpi.cc/2dqEMrU

![](img/d8322379620245fcd21eccc3855b8c78_76_0.png)

Above Surely it can't really be haunted... can it?

It’s late. The night grows dark and you’re near the end of your trick-or-treat run – but what’s this? A house you’ve never seen before on your road. Eager for more sweets, you make your way to the door. A haunted house sign greets you, but you think it merely decoration. Approaching the door, you press the doorbell – only for glass to break and the light to go out. A door creaks, the sign you had dismissed flashes, and you hear screams as the lights come back on. Startled, you look to your right and realise monsters are partying in the garage next to you, celebrating another victim in their night of ghastly fun.

“I had been trying to think of something fun to do for Halloween and the Raspberry Pi was an obvious choice,” the owner of this nightmarish house, Stewart, tells us. “I’d recently built a circuit for home automation using remote-control sockets and had the idea of using that to turn lights on and off automatically. I also spent some time looking around at shops to see what other Halloween props I could add to the project.”

The system is deceptively simple, although there’s a lot of different components to it. A dedicated doorbell is hooked up to a PiFace board that interacts directly with the Raspberry Pi and some Python code. Stewart chose the PiFace for this, instead of wiring up directly to the Raspberry Pi’s GPIO pins, to make sure he didn’t send any unwanted signals through the GPIO port – although with the right amount of research and careful wiring on a breadboard, you could do this without the PiFace. The PiFace directly controls the LEDs on a jack-o’-lantern and the haunted house sign, while the porch lights and the monster party lights are controlled by a wireless home-automation remote control that he directly soldered into.

“They are not designed to be used that way,” Stewart mentions. “The solder joints were not very reliable and whilst they lasted for Halloween, they eventually came loose. Since I made the project, Energenie have released a Pi-mote Raspberry Pi board for their remote-control sockets, which is easier to use than having to solder onto the remote-control buttons.”

Speakers play the appropriate sounds and music as dictated by the code, and the system is waterproofed as, well… Stewart lives in England. The results?

“I just managed to finish it in time for Halloween. We had friends coming over for our children’s Halloween party who said they liked it, and the trick-or-treaters were certainly surprised by it. There was one young girl who was a little scared by it, but most thought it was fun rather than scary.”

## COMPONENTS LIST
### CONJURE UP YOUR OWN VERSION

- PiFace
- Wi-Fi dongle (used during configuration and testing)
- 2× Reed relays (HE751A0G10) – one for the on button, the other for off
- 2× LEDs
- 2× AA batteries with holder
- Doorbell
- Remote control socket with spare remote
- Plastic light-up pumpkin lantern (with test button)
- Plastic haunted house sign
- Waterproof box (or lots of wire to allow the Raspberry Pi to be installed indoors)
- Speakers (with amplifier)

Below A simple box contains the Pi and all the electronics so that it's kept waterproof

![](img/d8322379620245fcd21eccc3855b8c78_76_1.png)
