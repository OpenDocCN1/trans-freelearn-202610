

## Static Batching summary

Static Batching is a powerful, but dangerous tool. If we don't use it wisely, we can very easily inflict enormous performance losses via memory consumption (potentially leading to application crashes) and rendering costs on our application. It also takes a good amount of manual tweaking and configuration to ensure that batches are being properly generated, and that we aren't accidentally introducing any unintended side effects of using various **Static** flags. However, it does have a significant advantage in that it can be used on meshes of different shapes and enormous sizes, which Dynamic Batching cannot provide.

For the most part, we should try to keep any meshes we want to be statically batched present in the original Scene file. However, if dynamic instantiation is necessary, or we are making use of additive Scene loading, then we can control static batch eligibility with the `StaticBatchUtility.Combine()` method. This utility method has two overloads: either we provide a root `GameObject`, in which case all child `GameObjects` with meshes will be turned into new static batch groups (multiple could be created if they share multiple Materials) or we provide a list of `GameObjects` and a root `GameObject`, and it will automatically attach them as children to the root and generate new static batch groups in the same manner.

We should profile our usage of this function, as it can be quite an expensive operation if there are many vertices to combine. It will also not combine the given meshes with any preexisting statically batched groups, even if they share the same Material. So we will not be able to save Draw Calls by instantiating or additively loading **Static** meshes that use the same Material as other statically batched groups already present in the Scene (it can only combine with meshes it was grouped with in the `Combine()` call).

Note that if any of the `GameObjects` we batch with the `StaticBatchUtility.Combine()` method are not marked as **Static** before batching, the `GameObjects` will remain non-**Static**, but the mesh itself will be **Static**. This means that we could accidentally move the `GameObject`, its `Collider` Component, and any other important objects, but the mesh will remain in the same location. Be careful about accidentally mixing **Static** and non-**Static** states in statically batched objects.

![](img/da79484c9b30dc915216dbc5454a9bbf_140_0.png)

## Summary

It is clear that the Dynamic Batching and Static Batching systems are not a silver bullet. We cannot blindly apply them to any given Scene and expect improvements. If our application and Scene happen to fit a particular set of parameters, then these methods are very effective at reducing CPU load and rendering bottlenecks. However, if not, then some additional work is required to prepare our Scene to meet batching feature requirements. Ultimately, only a good understanding of these batching systems and how they function can help us determine where and when this feature can be applied, and, hopefully, this chapter has given us all of the information we need to make informed decisions.

You will learn more about the Rendering Pipeline and performance improvement techniques in `Chapter 6, Dynamic Graphics`. Until then, let’s move onto a different topic and look into some of the more subtle performance improvements that we can achieve through managing our art assets in intelligent ways.

## 4 Kickstart Your Art

Art is a very subjective area, dominated by personal opinion and preference. It can be difficult to say whether, and why, one piece of art is better than the other, and it’s likely that we won’t be able to find complete consensus on our opinions. The technical aspects behind art assets that support a game’s artistry can also be very subjective. There are multiple workarounds that can be implemented to improve performance, but these tend to result in a loss of quality for the sake of speed. If we’re trying to reach peak performance then it’s important that we consult with our team members whenever we decide to make any changes to our art assets, as it is primarily a balancing act, which can be an art form in itself.

Whether we’re trying to minimize our runtime memory footprint, keeping the smallest possible executable size, maximizing loading speed, or maintaining consistency in frame rate, there are plenty of options to explore. There are some methods that are clearly always ideal, but most require a little more care and forethought before being adopted, as they would result in reduced quality or increase the chances of developing bottlenecks in other subsystems.

In this chapter, we will explore how to improve performance for the following asset types:

- Audio files
- Texture files
- Mesh and animation files
- Asset Bundles and Resources

In each case, we will investigate how Unity stores, loads, and manipulates these assets both during application build time and runtime. We will also examine our options in the event of performance issues, and what we can do to avoid behavior that might generate performance bottlenecks.

## Audio

Unity, as a framework, can be used to build anything from small applications that require only a handful of sound effects and a single background track to huge role playing games that need millions of lines of spoken dialog, music tracks, and ambient sound effects. Regardless of the actual scope of the application, audio files are often a large contributor to the application size after it is built (sometimes called its _disk footprint_). Moreover, many developers are surprised to find that runtime audio processing can turn into a significant source of CPU and memory consumption.

Audio is often neglected on both sides of the gaming industry; developers tend not to commit many resources to it until the last minute, whereas users rarely draw their attention to it. Nobody notices when audio is handled well, but we all know what bad audio sounds like--it's instantly recognizable, jarring, and guaranteed to draw unwanted attention. This makes it crucial not to sacrifice too much audio clarity in the name of performance.

Audio bottlenecks can come from a variety of sources. Excessive compression, too much audio manipulation, too many active Audio Components, inefficient memory storage methods, and access speeds are all ways to invite poor memory and CPU performance. However, with a little effort and understanding, all it takes is a few tweaks here and there to save us from a user experience disaster.

## Importing audio files

When we select an imported audio file in the **Project** window, the **Inspector** window will reveal multiple import settings. These settings dictate everything from loading behavior, compression behavior, quality, sample rate, and (in later versions of Unity) whether to support ambisonic audio (multichannel audio, which combines tracks via spherical harmonics in order to create more realistic audio experiences).

Many of the audio import options can be configured on a per-platform basis, allowing us to customize behavior between different target platforms.

![](img/da79484c9b30dc915216dbc5454a9bbf_143_0.png)

## Loading audio files

The following are the three settings that dictate the manner in which an audio file is loaded:

- Preload Audio Data
- Load In Background
- Load Type

Our audio files are initially packaged as binary data files that are bundled with our application, which reside on the hard disk of the device (although in some cases they downloaded from somewhere on the internet). Loading audio data simply means pulling it into main memory (RAM) so that it can be later processed by audio decoders, which then convert the data into audio signals to our headphones, or speakers. However, how loading happens will vary enormously based on the previous three settings. The first setting, Preload Audio Data determines whether audio data will be automatically loaded during Scene initialization, or loaded at a later time. When loading of audio data does occur, the second setting, Load In Background, determines whether this activity blocks the main thread until it is finished, or loads it asynchronously in the background. Finally, the Load Type setting defines what kind of data gets pulled into memory and how much data gets pulled at a time. All three of these settings can have a dramatically negative effect on performance if they are not used wisely.

The typical use case of an audio file is to assign it to the audioClip property of an AudioSource object, which will wrap it in an AudioClip object. We can then trigger playback via AudioSource.Play() or AudioSource.PlayOneShot(). Each Audio Clip assigned in this way would be loaded into memory during Scene initialization since the Scene contains immediate references to these files, which it must resolve before they are needed. This is the default case when Preload Audio Data is enabled.

Disabling Preload Audio Data tells the Unity Engine to skip audio file asset loading during Scene initialization, which defers loading activity to the first moment it is needed, in other words, when Play() or PlayOneShot() are called. Disabling this option will speed up Scene initialization, but it also means that the first time we play the file, the CPU will need to immediately access the disk, retrieve the file, load it into memory, decompress it, and play it. This is a synchronous operation and will block the main thread until it is completed. We can prove this with a simple test:

```csharp
public class PreloadAudioDataTest : MonoBehaviour {
[SerializeField] AudioSource _source;

void Update() {
if (Input.GetKeyDown(KeyCode.Space)) {
using (new CustomTimer("Time to play audio file", 1)) {

```

