Arguably, XR has recently been passing through this final phase and is starting to enjoy
much better support and much higher quality experiences than it did during the early days,
although it is true that the adoption rate of XR has been slower than initially predicted. It
remains to be seen whether XR will grow into a multi-billion dollar industry or fade into a
niche market of gadgets. Consequently, developing within this new medium is not without
its risks, and we can find industry analysts who will agree with our opinions, regardless of
where we stand on the future of XR. One thing is for certain, though, that every time
someone experiences firsthand what VR and AR are capable of, they're blown away by the
level of immersion and the medium's ability to convincingly transport them into another
world. This level of immersion and interactivity is unparalleled, and teases many more
possibilities as support for the platforms mature and technology continues to advance.

In this chapter, we will explore the following topics:

+   - What concerns to keep in mind when developing VR or AR projects in Unity and
+   what must be avoided

+   - Performance enhancements specific to the XR medium

# # XR Development

Developing an XR product in Unity involves importing one of several XR **Software**
**Development Kits (SDKs)** into our Unity project and making some specialized API calls to
configure and use the platform at runtime. Each SDK is different in its own way and offers a
different set of features. For instance, the Oculus Rift and HTC Vive SDKs provide APIs to
control VR HMDs and their respective Controllers, whereas Apple's ARKit provides
utilities to determine spatial positioning and superimpose objects on the display. Unity
technologies have been working hard to create APIs that support all of these variations, so
the APIs for XR development in Unity have changed a lot over the past couple of years.

The early days of Unity VR development meant dropping Native Plugins into our Unity
projects, importing SDKs directly from an external developer portal, and all kinds of
annoying grunt-work in setup, and applying updates manually. Since then, however, Unity
has incorporated several of these SDKs directly into the Editor. In addition, since AR has
become more popular recently, the main API has been renamed
from `UnityEngine.VR` to `UnityEngine.XR` in Unity 2017.2.0 and greater, in order to be
inclusive of several AR SDKs. Importing various XR SDKs and configuring them can be
managed through the **Edit | Project Settings | Project | XR Settings** area.