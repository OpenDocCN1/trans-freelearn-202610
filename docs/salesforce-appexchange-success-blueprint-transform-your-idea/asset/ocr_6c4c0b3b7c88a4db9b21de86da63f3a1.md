To leverage Platform Cache in your managed packages, define namespaced cache partitions in your package and reference them in your code. Allocate the free cache to the desired partitions and upload a new version of your application. For existing packages that have already passed the security review prior to the Spring ’21 release, the cache will automatically be provisioned upon installation when the allocation is set in the managed package.

# Improving managed package performance with Platform Cache

Platform Cache can be particularly beneficial for managed packages that rely heavily on data retrieval operations, as it can help to reduce latency and improve overall performance. For example, consider a managed package that frequently accesses configuration data or frequently used metadata. By storing this data in Platform Cache, the managed package can access it more quickly than by querying the database each time the data is needed. This can lead to faster response times, improved application performance, and a better UX for customers.

# Case study – enhancing performance in Formula Debugger with Platform Cache

If you’re still scratching your head over how to use Platform Cache, fret not! We’re about to shine a light on this subject by delving into how Platform Cache was leveraged in Formula Debugger, an app we took for a spin back in