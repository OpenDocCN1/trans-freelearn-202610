The code for the Xamarin IDE is the exact same as the code written for Visual Studio, so the same C# code comes along:

```
...
namespace firstclojure
{
    class MainClass
    {
        public static void Main (string[] args)
        {
            hello.net fromclojure = new hello.net();
            string sunset = fromclojure.getsunset("Tokyo");
            Console.WriteLine("Sunset today in Tokyo is {0}",
            sunset);
            Thread.Sleep (2000);
        }
    }
}
```