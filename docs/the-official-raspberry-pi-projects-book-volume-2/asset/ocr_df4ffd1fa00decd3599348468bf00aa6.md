![](bbox=[0.06, 0.13, 0.45, 0.38])

![](bbox=[0.55, 0.13, 0.95, 0.38])

to be played (also a string). A simple way is to use a Python construct called a dictionary. A real-world dictionary has an index of words, and each word has definitions. In a Python dictionary, the word is called the ‘key’, and the definitions the ‘values’. Listing_l2.py uses a simple two-item dictionary to map sounds and names (the values) onto the C and D keys (the keys). Give it a try.

# >STEP-06

# Putting it all together

We’ve now explored everything needed for our relative pitch tester: we’ll use the piano sounds that come with the Piano-HAT library for our notes. Type up the code from the Relative_Pitch.py listing (on page 51) and run it with sudo. A reference

# Listing_l1.py

```
import pianohat
import pygame
import signal

pygame.mixer.pre_init(44100, -16, 1, 512)
pygame.mixer.init()
pygame.mixer.set_num_channels(16)

def handle_note(channel, pressed):
    if channel == 2:
        pygame.mixer.Sound('./mysound.wav').play(loops=0)

pianohat.on_note(handle_note)
signal.pause()
```

> In a Python dictionary, the word is called the ‘key’, and the definitions the ‘values’

note (a C) is played, then, after a pause, the note to be identified. The player then has 6 seconds to press the correct key for the note they just heard. If they get it right, all the LEDs will flash in celebration, otherwise they’re asked to try again. As an extension, how about using different, selectable instruments?

# Listing_l2.py

```
import pianohat
import pygame
import signal

pygame.mixer.pre_init(44100, -16, 1, 512)
pygame.mixer.init()
pygame.mixer.set_num_channels(16)
NOTES = {0:['Sound1', './mysound.wav'],
         2: ['Sound2', './mysound2.wav']}

def handle_note(channel, pressed):
    if channel == 0 or channel == 2:
        pygame.mixer.Sound(NOTES[channel][1]).play(loops=0)

pianohat.on_note(handle_note)
signal.pause()
```