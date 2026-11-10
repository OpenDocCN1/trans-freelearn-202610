
## MAGIC MIRROR

Mirror mirror on the wall, what’s the weather going to be like today? Should I bring an umbrella?

![](img/d8322379620245fcd21eccc3855b8c78_55_0.png)

BRADLEY MELTON
A network engineer and aspiring ‘professional geek’ who loves to use and sing the praises of the Raspberry Pi.
imgur.com/A4kjx7w

### Quick Facts

- The project is made with Google Code
- It took a week to do the frame, a few days for the code
- This is Bradley’s first creative Pi project
- In the past, he has used Pis in his home Cisco networking lab
- The online community helped a lot to get this to work

It’s amazing how the Iron Man films have inspired people due to the way they portrayed almost-attainable technology. Watching someone look over sunny Malibu while the weather info was displayed right in front of them was a great visual. It’s not exactly brand new, unseen technology – cars have been projecting HUDs onto windscreens for a while now – but it has never been popularised in a major blockbuster before. While we don’t seem to have quite reached the stage of the incredible glass tech of Tony Stark’s bedroom just yet, apparently we’re close enough to get mirrors working in the same way, or at least Bradley Melton has managed it with his Magic Mirror.

“It’s called a ‘Magic Mirror’, but a more accurate name would be a ‘Smart Mirror’,” Bradley tells us. “It’s a mirror that displays the information you need to know at a quick glance: the time, the date, the weather, and of course a compliment!”

It’s not the first mirror of its type, and Bradley admits that he’s taken some cues from a previous project by Michael Teeuw (see more details about it on Michael’s blog: magpi.cc/1PzFbWa), taking the concept and bringing it down to a more beginner level for himself so he could learn more about web development.

“Plus, because I know what each and every function does and how it works, it makes it easier to fix bugs as well as make improvements in the future.”

It’s an extra-useful mirror as it displays up-to-date weather information taken from the internet

It looks like a mirror, but it’s actually a cunning disguise – a monitor, and some two-way glass covered by a frame

![](img/d8322379620245fcd21eccc3855b8c78_55_1.png)

Of course, a mirror should also be able to reflect your image and make sure you’re looking good

### A TECH REFLECTION

![](img/d8322379620245fcd21eccc3855b8c78_56_0.png)

![](img/d8322379620245fcd21eccc3855b8c78_56_1.png)

![](img/d8322379620245fcd21eccc3855b8c78_56_2.png)

### >STEP-01 Turn the mirror on

Turning the display on activates the Raspberry Pi, which is powered via a USB port on the mirror itself. It boots into the Chromium web browser, which is displayed on the screen.

### >STEP-02 Getting the data

The weather info is taken from Yahoo, scraped using the simpleWeather.js jQuery plug-in which then displays the weather on the display. Time is displayed as well, along with a compliment.

### >STEP-03 Keep me updated

Everything works on a timer, so the weather and time are updated on the browser window that makes up the display every 15 seconds, while the compliment is updated every 30 seconds.

Rather than use an actual mirror and project the data upon it, the Magic Mirror uses a widescreen monitor that has been put in a portrait orientation with an acrylic two-way mirror on top. With the right lighting and display tweaks, it can be reflective enough to use as a mirror while also displaying the weather data. The electronics are fairly simple: it’s just a Pi with HDMI linking to the mirror, a WiFi dongle to retrieve online data, and a USB cable to the monitor as well, which is how it draws its power. To finish it off, Bradley built a wooden frame to be laid over the bezel so that the whole thing was camouflaged a bit better.

“It’s not very complex,” Bradley informs us. “As long as you have a little bit of carpentry or DIY skill to build the frame and have a basic understanding of how to program, you should be able to build this. I have never used JavaScript or CSS before this project, and I only had a little bit of experience with HTML, but this webpage is built almost entirely from JavaScript and CSS.” At the time of writing, the mirror has been running for a few weeks without any problems. It seems like Bradley wants to include some holiday-themed extras to it, starting with some spooky additions for the Halloween just past. We’re hoping he will add some jollier ones for the Christmas period.

> Rather than use an actual mirror and project data upon it, the Magic Mirror uses a widescreen monitor

![](img/d8322379620245fcd21eccc3855b8c78_56_3.png)

Above Say 'Bloody Mary' three times into the mirror on Halloween for a voice-activated fright

## ADRIAN ATWOOD

Dad, engineer, tinkerer, woodworker, borker, and crazy hobbyist who is always building weird projects.
buildxyz.xyz

![](img/d8322379620245fcd21eccc3855b8c78_57_0.png)

![](img/d8322379620245fcd21eccc3855b8c78_57_1.png)

Turn the system on with a code and set a timer, giving you full parental control

A standard LCD screen is built into this beautiful cabinet, and displays Kodi or Emulation Station from the Pi

The many USB ports allow for controllers and USB storage drives; however, media is accessible over the network as well

### Quick Facts

- It took 90 hours to build
- This is Adrian's first Pi project
- Scratch is installed to encourage the kids to code
- He's adding wireless PS2 controllers very soon
- Adrian has more cool Pi stuff planned – keep an eye on his blog

## RPIKIDS

Not a spin-off Saturday morning cartoon, RPiKids is a new entertainment system built for Adrian Atwood’s children

We’ve all seen wacky parental control methods for kids’ entertainment. Our favourite is the exercise bike that powers a PlayStation, which may seem cruel but at least means there’s a cut-off point for how much they’re able to play. The best method is still the simplest: a key code and a timer so that the kids can then do something other than stare at a screen all day, freeing up the TV so you can binge-watch the entirety of Once Upon a Time’s third series. With the Raspberry Pi being a far more powerful option for operating an entertainment centre, it was only a matter of time before an intrepid hacker made the ultimate version for their kids. Meet RPiKids, created by Adrian Atwood.

“RPiKids is an all-in-one kids’ entertainment centre with integrated parental control features,” Adrian explains. “My kids can enjoy 16-bit games, watch movies and more, provided an adult has granted them access by entering a password and setting the timer.”

The RPiKids is a mix of hardware; the system itself is powered by a Raspberry Pi, handling Kodi, Emulation Station and the like for the entertainment side. The passcode and timer are handled by an Arduino Uno using a rotary encoder (think bank safe), which can be pushed as a button to confirm selections. It’s all built into a wonderful-looking, steampunky case that was handcrafted by Adrian himself from wood with 3D printed detailing.

There are USB access ports on the front for connecting controllers and portable media storage, and the hardware is visible behind a removable clear Perspex panel. While it all looks extremely impressive, Adrian claims it’s an overall ‘intermediate’ level of build.

“I will say the Raspberry Pi configuration was very straightforward and a beginner can mirror my configuration,” he tells us. “Coding the firmware for the Arduino parental controls, and having the Arduino and Pi 2 communicate with each other, was a little more complex. The case is no problem if you’re into woodworking and auto body repair. I have access to a 3D printer and laser cutter and enjoy CAD modelling, but those parts could be contracted out or replaced.”

### ENTERTAINING THE KIDS

The gear that powers the centre is all visible through this Perspex sheet – good for maintenance and inquisitive minds

![](img/d8322379620245fcd21eccc3855b8c78_58_0.png)

![](img/d8322379620245fcd21eccc3855b8c78_58_1.png)

### >STEP-01 Dial it up

Using a digital encoder and a NeoPixel LED ring, the user (parent) enters a combination to unlock the system so the children can use it.

![](img/d8322379620245fcd21eccc3855b8c78_58_2.png)

### >STEP-02 Set a time

When unlocked, the encoder can then be used to set a timer on the system, from ten minutes to forever. Once selected, the Arduino lets the Raspberry Pi turn on.

![](img/d8322379620245fcd21eccc3855b8c78_58_3.png)

### >STEP-03 Shut it down

When time is up (or the off button is pressed), the Arduino part issues a shutdown command to the Pi. The Pi then shuts everything off safely, and the Arduino cuts power shortly afterwards.

> RPiKids is an all-in-one kids’ entertainment centre with integrated parental control

Although he expected ‘all sorts’ of glitches, the system has worked extremely well for him and his children, requiring only minor adjustments to the brightness of the ring of NeoPixels used in conjunction with the encoder.

One thing Adrian recommends for anyone trying to replicate the project is an upgrade to the sound output: “The Pi definitely needed a DAC (I²S) sound card because there was a lot of hissing and artifacts with the onboard sound.”

The kids have been very much enjoying it. Although it’s an entertainment centre, he’s also programmed an intentional security vulnerability into the lock mechanism, hoping his kids will learn something about security systems. We wish them the best of luck in figuring out to unlock ‘unlimited mode’!
