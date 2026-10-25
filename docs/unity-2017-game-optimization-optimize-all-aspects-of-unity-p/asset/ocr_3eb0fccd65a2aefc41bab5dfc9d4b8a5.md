However, before we spend even a single moment making performance fixes, we will need to prove that a performance problem exists to begin with. It is unwise to spend time rewriting and refactoring code until there is good reason to do so, since pre-optimization is rarely worth the hassle. Once we have proof of a performance issue, the next task is figuring out exactly where the bottleneck is located. It is important to ensure that we understand why the performance issue is happening, otherwise we could waste even more time applying fixes that are little more than educated guesses. Doing so often means that we only fix a symptom of the issue, not its root cause, and so we risk the chance that it manifests itself in other ways in the future, or in ways we haven't yet detected.

In this chapter, we will explore the following:

+   - How to gather profiling data using the Unity Profiler
+   - How to analyze Profiler data for performance bottlenecks
+   - Techniques to isolate a performance problem and determine its root cause

With a thorough understanding of a given problem, you will then be ready for information presented in the remaining chapters, where you will learn what solutions are available for the issue we've detected.

# # The Unity Profiler

The Unity Profiler is built into the Unity Editor itself and provides an expedient way of narrowing down our search for performance bottlenecks by generating usage and statistics reports on a multitude of Unity3D subsystems during runtime. The different subsystems it can gather data for are listed as follows:

+   - CPU consumption (per-major subsystem)
+   - Basic and detailed rendering and GPU information
+   - Runtime memory allocations and overall consumption
+   - Audio source/data usage
+   - Physics Engine (2D and 3D) usage
+   - Network messaging and operation usage
+   - Video playback usage
+   - Basic and detailed user interface performance (new in Unity 2017)
+   - Global Illumination statistics (new in Unity 2017)