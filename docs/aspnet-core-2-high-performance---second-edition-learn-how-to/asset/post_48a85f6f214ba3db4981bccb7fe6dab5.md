
## Statistics

When interpreting your collected metrics, it helps to know some basic statistics in order to read them correctly. Taking a simple *mean* average can mislead you and may not be as important as some other characteristics.

When instrumenting your code to collect metadata, you should have a rough idea of how often particular logging statements will be called. This doesn't have to be exact, and a rough order of magnitude approximation (or **Fermi estimate**) will usually suffice.

The question you should try to answer is how much of the data should be collected—all of it or a random sample? If you need to perform sampling, then you should calculate how big the sample size should be. We covered sampling in relation to SQL in `Chapter 7`, *Optimizing I/O Performance*, and the idea is similar here. Performance statistics require the same level of rigor as benchmarking does, and you can easily be misled or draw incorrect conclusions.

StatsD includes built-in support for sampling, but there are many other approaches available if you want to investigate them. For example, online streaming algorithms and reservoirs are two options. The important thing to keep in mind for performance is to use a fast **random number generator (RNG)**. As, for sampling, this doesn't need to be cryptographically secure; a **pseudorandom number generator (PRNG)** is fine. In .NET, you can use `new Random()` for a PRNG rather than the more secure option of `RandomNumberGenerator.Create()`. See `Chapter 8`, *Understanding Code Execution and Asynchronous Operations*, for more examples of how to use both of these.

When looking at your results, the outliers may be more interesting than the average. Although the median is more valuable than the mean, in this case, you should really look at the percentiles, for example, the 90th, 95th, and 99th percentiles. These data points can represent only a small fraction of your data, but at scale, they can occur frequently. You want to optimize for these worst case scenarios because if your users experience pages loads taking over five seconds 10% of the time (even though the average looks fast), then they may go elsewhere.

There's much more to say about statistics, but beyond the basics there are diminishing returns. If your math is rusty, then it is probably wise to have a refresher (Wikipedia is great for this). Then, you can explore some more advanced techniques, for example, the high-performance **HyperLogLog (HLL)** algorithm, which can estimate the size of a large set of elements using very little memory.

Redis supports the HLL data structure (with the PFADD, PFCOUNT, and PFMERGE commands). For more on different data structures, refer to Chapter 8, Understanding Code Execution and Asynchronous Operations.

![](img/5e416ac29f1bccaad147c6829853fd77_286_0.png)

This is only a brief introduction to performance logging, but there is much more for you to explore. For example, if you want to standardize your approach, then you can look into APDEX (http://www.apdex.org/), which sets a standard method to record the performance of applications and compute scores.

## Managing stale caches

It's worth providing a quick reminder to still consider simple issues after all of this complexity. It is far too easy to get lost in the details of a complicated bug or performance tweak and miss the obvious.

![](img/5e416ac29f1bccaad147c6829853fd77_286_1.png)

A good technique to help with this is rubber duck debugging, which gets its name from the process of explaining your problem to a rubber duck on your desk. Most of us have experienced solving a problem after asking for help, even though the other person hasn't said anything. The process of explaining the problem to someone (or something) else clarifies it, and the solution becomes obvious.

If something appears to not be working after a fix, then check simple things first. See whether the patch has actually been delivered and deployed. You may be seeing stale code from a cache instead of your new version.

When managing caches, versioning is a useful technique to help you identify stale assets. You can alter filenames or add comments to include a unique version string. This can be Semantic Versioning (SemVer), an ISO date and timestamp, or a hash of the contents. For more on cache busting, refer to Chapter 9, Learning Caching and Message Queuing.

![](img/5e416ac29f1bccaad147c6829853fd77_286_2.png)

SemVer is a great way of versioning your code because it implicitly captures information on compatibility and breaking changes. You can read more about SemVer at semver.org.

## 11

## Monitoring Performance Regressions

This chapter will cover writing automated tests to monitor performance, along with adding the tests to a **Continuous Integration (CI)** and deployment system. By constantly checking for regressions, you'll avoid building a slow application accidentally. We'll also cover how to safely load test a system without forcing it offline and how to ensure that the tests mimics real-life usage as far as possible.

Topics covered in this chapter include the following:

- Profiling
- Load testing
- Automated testing
- Performance monitoring
- Continuous integration and deployment
- Realistic environments and production-like data
- UI testing with selenium and phantom headless browsers
- A/B testing for conversion optimization
- Cloud services and hosting
- DevOps

You will see how to automate performance monitoring and testing so that you don't need to remember to keep doing it manually. You'll learn how to catch regressions early, before they cause trouble, and how to safely back them out for rework.

NuGet packages are a great way of managing internal projects. In Visual Studio 2017, you can see the source code of packages and debug into them. This means no more huge solutions, containing a ludicrous number of projects that are hard to navigate and slow to build in Visual Studio.

Bypassing processes and doing things manually will cause problems even if you are following a script. If it can be automated, then it probably should be, and this includes testing.

## Testing

Testing is essential to producing high-quality and well-performing software. The secret to productive testing is to make it easy, reliable, and routine. If testing is difficult or tests regularly fail because of issues unrelated to the software (for example, environmental problems), then tests will not be performed or the results will be ignored. This will cause you to miss genuine problems and ship bugs that you could have easily avoided.

There are many different varieties of testing, and you may be familiar with the more common cases used for functional verification. In this book, we will mainly focus on tests pertaining to performance. However, the advice here is applicable to many types of testing.

## Automated testing

As mentioned previously, the key to improving almost everything is automation. Tests that are only run manually on developer workstations add very little value. Of course, it should be possible to run the tests on desktops, but this shouldn't be the official result because there's no guarantee that they will pass on a server (where the correct functioning matters more).

Although automation usually occurs on servers, it can be useful to automate tests that run on developer workstations too. You can do this live unit testing in Visual Studio 2017 Enterprise Edition, and in other versions, you could use a plugin such as NCrunch. This runs your tests as you work, which can be very useful if you practice test-driven development (TDD) and write your tests before your implementations.

One way of enforcing testing is to use gated check-ins in TFS, but this can be a little draconian, and if you use an SCM such as Git, then it's easier to work on branches and simply block merges until all the tests pass. You want to encourage developers to check-in early and often because this makes merges easier.
