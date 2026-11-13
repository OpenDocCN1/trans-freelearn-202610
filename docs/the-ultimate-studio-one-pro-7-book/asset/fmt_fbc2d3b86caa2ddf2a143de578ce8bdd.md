
5. You can drag pedals around the pedalboard to change their order in the signal chain.

`Ampire` is tons of fun, but it is one of the more CPU-hungry plug-ins in Studio One's arsenal. That's quite understandable, considering all the modeling technology under the hood. But you may want to keep an eye on the performance indicator if you have several guitar tracks with `Ampire` running on all of them. If you start hearing clicks and dropouts, increase the `Audio Dropout Protection` setting, as we discussed in [*Chapter 11*](#B31512_11.xhtml#_idTextAnchor181).

### Bitcrusher

`Bitcrusher` represents the extreme end of the distortion spectrum. It's the plug-in you'll reach out for when you want to obliterate your sound. With controls such as `Wreck`, which lets you specify how much you want to degrade the sound by reducing its bit depth, and `Dirt`, which introduces instability to higher frequencies to create an aggressive effect, you can be sure that this thing will not be gentle.

### Red Light Distortion

`Red Light Distortion` is a very flexible plug-in that emulates analog distortion with a very wide range of flavors. Use the `Type` drop-down menu to select the distortion type and use the other settings to fine-tune the level of distortion. `Red Light Distortion` can be very gentle; use it to add sparkle to liven up a dull vocal recording. It can also be very bold; use it to make a sound appear as though it's coming from a broken radio. Feel free to experiment, but keep the volume down when experimenting. Distortion can suddenly make things very loud!

## Modulation plug-ins

`Modulation effects` are a group of audio effects used in music production to create movement, depth, and interest in sound. These effects can be applied to a variety of elements in a mix, including instruments, vocals, and drums. Modulation effects help you to create unique and expressive sounds that make your music stand out.

Studio One Pro comes bundled with seven modulation effect plug-ins. Let's take a look at them one by one.

### Autofilter

`Autofilter` has two independent frequency filters that can be controlled and programmed in a variety of ways. You can tweak the filters manually, control them with a built-in low-frequency oscillator, program them with a 16-step sequencer, or lock them to your project's tempo to create anything from DJ-style filter sweeps to complex rhythmic patterns.

It's a bit of a learning curve, but certainly worth the effort if you're producing EDM or similar genres.

### Chorus

`Chorus` is an effect that creates a thick, rich sound by duplicating a signal and adding slight variations to its pitch and time. It can be used to add warmth and depth to a sound, as well as to create a sense of space and movement.

Studio One's `Chorus` plug-in has a retro-style interface, which reflects the fact that it actually models vintage analog chorus devices. It adds a warm analog color to the sound and lets you quickly dial in the rich chorus tones prized in many music genres.

### Flanger

`Flanger` is a similar effect to chorus but with a more pronounced sweeping sound. Flangers create a swirling, jet-like sound by combining two identical signals that are slightly delayed and then modulated with a filter. Flanging is most commonly used on electric guitars but is equally at home on synth tracks as well.

Flangers are simple devices with just a few controls, and Studio One's `Flanger` plug-in is no exception. One feature worth noting is that it can be synchronized to your project's tempo, so you can set its dramatic sweeping sounds to make more rhythmic sense.

### Phaser

A `phaser` creates a series of notches and peaks in a sound by splitting the signal into two paths, delaying one path, and modulating it with a filter. The result is a unique sound that creates a sense of movement and depth, which can be anywhere from subtle to psychedelic.

Studio One's `Phaser` plug-in is a very simple device that offers the capability to lock its sweeps to your project's tempo.

### Rotor

`Rotor` emulates the sonic characteristics of rotating speaker cabinets. These cabinets are traditionally paired with Hammond organs and are responsible for that instrument's unique sound, but having this plug-in at your fingertips will allow you to try it on anything – it sounds great on electric guitars and synthesizers, for example.

This plug-in is an accurate model of the real thing, so much so that you can even see the virtual speakers rotating inside the cabinet, and their spinning speed changes based on your settings. Go ahead and play with this plug-in. It's tons of fun and sounds great.

### Vocoder

A `vocoder` analyzes and synthesizes the spectral and temporal characteristics of human speech and applies them to another audio signal. Here's how it works: a vocoder takes a modulator signal (usually a human voice) and a carrier signal (usually a musical instrument) and combines them to create a unique, synthesized sound (which usually sounds like a singing robot). Vocoders are used in a wide variety of genres, from electronic to hip-hop to film music and sound design.

Studio One's `Vocoder` plug-in is a powerful tool that allows you to come up with some very creative sounds and textures. Setting up `Vocoder` is a bit different compared to the other effects plug-ins, so let's walk you through it:

1. Open the `Browse` panel and go to the `Effects` tab.
2. Find `Vocoder`, then drag and drop it on the track you'd like to process. The audio on this track will be `Vocoder`'s modulator signal. Here's what `Vocoder` looks like:

![Figure 12.10: Vocoder](image/B31512_12_10.jpg)

Figure 12.10: Vocoder

3. Now, let's take care of the carrier signal. `Vocoder` can use audio from another track as the carrier, or create a carrier signal using its own oscillator. If you'd like it to use audio from another track, click the `Sidechain` button on top of the `Vocoder` window, and click on the `Send` column corresponding to the track you'd like to use as the carrier signal:

![Figure 12.11: Selecting a carrier signal for Vocoder](image/B31512_12_11.jpg)

Figure 12.11: Selecting a carrier signal for Vocoder

4. If you'd like `Vocoder` to use its own oscillator as the carrier signal, use the internal carrier settings to set the oscillator to the desired waveform and frequency.
5. Now it's time to play! The `Patch Matrix` section in the middle of the `Vocoder` window sets how the `Vocoder` analyzer section is routed to its synthesizer section. This routing is displayed by the small red "pins" on the matrix. By default, these sections are matched sequentially, which results in the most intelligible vocoded voice, but you can change the position of the pins to modify the routing and come up with some otherworldly effects. You can nerd up on the science behind this, but it's much faster, and definitely more fun, to learn how it works by playing with it.
6. Vocoding works best with vowels. Consonants don't perform that well. The `Unvoiced Replacement` section on the right side of the window controls an algorithm that detects consonants in the modulator signal and replaces them with other signals to increase speech intelligibility. The success of this algorithm depends on source material, so there is no general recipe. As always, experimentation is key.

Of all the effects plug-ins, `Vocoder` is probably the most creative and fun. It allows you to morph two sounds into one, creating an instrument that seems to talk or a singer that seems to be controlled by a MIDI keyboard. There's a lot of creative potential waiting to happen, so it's definitely worth a serious look.

### X-Trem

`X-Trem` is a tremolo effect. `Tremolo` creates a pulsating or rhythmic sound by modulating the volume of a signal. It can be used to add a sense of motion or rhythmic quality.
