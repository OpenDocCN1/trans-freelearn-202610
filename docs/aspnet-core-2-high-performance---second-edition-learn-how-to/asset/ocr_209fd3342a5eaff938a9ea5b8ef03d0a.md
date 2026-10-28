You can enable these features with the AsParallel() method. The ForAll() method has the same purpose as the loops in previous examples and is useful if the order is unimportant. If ordering is important, then there is an AsOrdered() method that can help solve this. However, this can reduce the performance gains due to the extra processing involved.

This example performs similarly to the previous one that used a parallel loop, which is unsurprising. We can also limit the number of operations that can occur in parallel, using the WithDegreeOfParallelism() method as follows:

```
passwords.AsParallel().WithDegreeOfParallelism(2).ForAll(p =>
{
    var pbkdf2 = new Rfc2898DeriveBytes(p, 256, 10000);
    Convert.ToBase64String(pbkdf2.GetBytes(256));
});
```

This preceding example limits the hashes to two at a time and performs similarly to when we only had two passwords in the list, which is to be expected. This can be useful if you don't want to max out the CPU, because there are other important processes running on it.

You can achieve the same effect with the TPL by setting the MaxDegreeOfParallelism property on an instance of the ParallelOptions class. This object can then be passed into overloaded versions of the loop methods as a parameter, along with the main body.

![](bbox=[0.12, 0.64, 0.22, 0.73])

It's important, when you're using parallel LINQ to query datasets, that you don't lose sight of the best place for the query to be performed. You may speed up a query in the application with parallelization, but the best place for the query to occur may still be in the database, which can be even faster. To read more on this topic, refer back to Chapter 7, Optimizing I/O Performance, and Chapter 5, Fixing Common Performance Problems.

# Parallel benchmarking

Let's have a look at the output of a simple .NET Core console application, which benchmarks the techniques that we have just discussed. It shows one situation where parallelization doesn't help and actually makes things worse.