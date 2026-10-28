You could improve the benchmark by warming up the function under test, then running a garbage collection, before starting the timer and entering the loop. You could also store the output in a member variable to prevent clever optimizations by the compiler.

However, at this point, it would be better to just use a full benchmarking tool, such as BenchmarkDotNet. The reason we haven't done so here is simply to make it easier for you to run these samples for yourself.

BenchmarkDotNet is a very helpful tool that has now joined the projects of the .NET Foundation. We will cover it again in Chapter 8, Understanding Code Execution and Asynchronous Operations. You can read more about it at https://github.com/dotnet/BenchmarkDotNet.

![](bbox=[0.12, 0.35, 0.19, 0.44])

In these examples, we are using C# 6 string interpolation, but you can use the traditional overloads to Console.WriteLine if you prefer.

Let's write a quick example application that demonstrates the differences by running these two different versions multiple times. We'll extract the two tests into methods and call them each a few times:

```
var pbkdf2 = new Rfc2898DeriveBytes("password", 64, 256);
SingleTest(pbkdf2);
SingleTest(pbkdf2);
SingleTest(pbkdf2);

Console.WriteLine();
var tests = 1000;
AvgTest(pbkdf2, tests);
AvgTest(pbkdf2, tests);
AvgTest(pbkdf2, tests);

Console.WriteLine();
Console.WriteLine("Press any key...");
Console.ReadKey(true);
```