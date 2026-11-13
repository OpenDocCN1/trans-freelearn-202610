
This technology is used in various applications, from music and film to **virtual reality** (**VR**), **augmented reality** (**AR**), and gaming. Immersive audio formats such as Dolby Atmos, DTS:X, and Ambisonics are designed to deliver spatially dynamic audio experiences on multiple playback systems, from professional setups to consumer-grade headphones. By mimicking how we naturally perceive sound in the real world, immersive audio allows creators to craft more engaging and lifelike soundscapes that captivate and transport the listener.

In practical terms, spatial audio refers to any audio format with output channels beyond traditional two-channel stereo. This is indeed a broad definition, so let’s break it down.

# Understanding different types of spatial audio

Spatial audio formats supported by Studio One include the following:

*   `Surround` is a general term that refers to multi-channel audio configurations where each sound is assigned to a particular channel and can be positioned anywhere around the listener using the pan control on that channel. Commonly used in home theaters, cinemas, and gaming, surround audio formats such as 5.1 and 7.1 channel setups are designed to replicate real-world acoustics.

*   `Dolby Atmos` is a proprietary spatial audio format developed by Dolby Labs. It takes a more advanced and flexible approach by allowing sounds to be placed and moved anywhere in a three-dimensional space, including overhead. Dolby Atmos is scalable, meaning it can adapt to systems ranging from cinema setups with hundreds of speakers to soundbars and headphones.

*   `Binaural` is a two-channel audio signal that carries spatial information and can be used to convey spatial audio imaging on headphones without the need for a multi-speaker installation.

These formats, with the exception of binaural, require the use of several speakers both during production and during playback. Once we go beyond stereo, where we simply name our speakers left and right, naming our speakers can become a bit more tricky, so let’s take care of that before we dive into the deep end of spatial audio.

## The naming convention for speaker configurations

Spatial audio speaker configurations describe the placement and number of speakers in a setup designed to create an immersive audio experience. These configurations are expressed as numbers such as 5.1, 7.1.2, and so on, which indicate the distribution of speakers, subwoofers, and, in some cases, the presence of height or overhead channels.

Here’s how to decipher a number such as 7.1.2:

*   The first number is the number of speakers placed around the listener on a horizontal plane. These are typically positioned at the front, sides, and rear.

*   The second number shows the number of subwoofers dedicated to **low-frequency effects** (**LFEs**), such as bass and rumble.

*   The third number, if present, indicates the number of **height** or overhead speakers that provide a vertical dimension to the audio, crucial for formats such as Dolby Atmos.

Here are some common configurations and their typical use cases:

*   **2.1**: Two main speakers (left and right) and one subwoofer. This is a basic stereo setup with added bass support, offering no surround or height channels.

*   **5.1**: Five main speakers (front left, front right, center, rear left, and rear right) and one subwoofer. This is the standard surround sound setup used in home theaters.

*   **7.1**: Adds two additional speakers (surround left and surround right) to the 5.1 setup for more precise sound placement around the listener.

*   **5.1.2**: A 5.1 setup with two height speakers, adding an overhead dimension. Ideal for entry-level Dolby Atmos or DTS:X systems.

*   **7.1.2**: A 7.1 setup with two height speakers. This configuration provides both horizontal and vertical immersion, which is suitable for advanced spatial audio experiences.

*   **7.1.4**: Four height speakers added to a 7.1 configuration, offering even more precise vertical sound placement for a highly immersive experience.

The sheer number of speakers mentioned in these configurations may be intimidating at first: do you really need that many speakers positioned in your home studio to work with spatial audio? That depends on what you’re planning to do and your target audience. That’s what we will discuss in the next section.

## Hardware requirements for spatial audio

Let’s start with the good news – if you’re going to work in Dolby Atmos, you don’t need anything more than your existing sound interface and headphones. Dolby Atmos uses a proprietary algorithm called the `Dolby Atmos Renderer`, which we will discuss in the *Mixing and exporting spatial audio material* section. Suffice it to say for now that this renderer will downmix your Dolby Atmos material and convert it to a binaural signal so that you can work with your headphones and still experience spatial audio.

And now the bad news – if you’re working in any other spatial audio format, including traditional surround formats such as 5.1, Dolby Atmos Renderer will not be able to apply its magic. So, you’ll need to have as many speakers as indicated by your surround configuration (e.g., five speakers and one subwoofer for a 5.1 configuration). What’s more, you’ll also need an audio interface that supports the same number of discrete outputs (e.g., a total of six outputs for a 5.1 configuration).

Now that we have an understanding of spatial audio formats and the requirements for each, we are ready to start working on our first immersive audio masterpiece in Studio One, and that’s exactly what we’re doing in the next section.

# Creating a new Song for spatial audio

In Studio One, spatial audio is only supported on the `Song` page. You’ll create, edit, mix, and export all your spatial material using this page only. Let’s get started:

1.  On Studio One’s start page, click `New`. This will open the `New` document window.

2.  On the left side of this window, in the `Templates` list, click on the template titled `Mix in Surround`. The right side of the window will now display the options available for creating your first Song using spatial audio (*Figure 14.1*).

![Figure 14.1: Creating a new Song for spatial audio](image/B31512_14_1.jpg)

Figure 14.1: Creating a new Song for spatial audio

1.  Select the type of spatial audio format you’d like to work in: `Dolby Atmos` or `Surround`. You can also select `Tutorial` to enter an interactive guide designed to help you learn about the Dolby Atmos workflow.

The options you’ll see on the right side of the `New` document window will change depending on your selection of `Dolby Atmos` and `Surround`. Let’s go through them one by one.

The options to create a Song for `Dolby Atmos` (shown in *Figure 14.1*) are as follows:

*   `Name`: Provide a name and pick a location for your Song.

*   `Bed Format`: Choose your target speaker configuration here. This is the speaker configuration that your project will ultimately play on.

*   `Monitoring Format`: This is the speaker configuration that you’ll be using while you do your production work. Remember we talked about the Dolby Atmos Renderer in the *Hardware requirements for spatial audio* section? That renderer will take the configuration you selected for `Bed Format` and downmix it to whatever speaker configuration you have selected here. If you’re just going to be working with headphones, select `Stereo` in this field.

*   `Sample Rate`: Dolby Atmos only supports sample rates of 48 kHz or 96 kHz, and those are the options you’ll see here. If your audio interface does not support 96 kHz, the only available option will be **48 kHz**.

The options to create a Song for `Surround` are as follows:

*   `Name`: Provide a name and pick a location for your Song.
