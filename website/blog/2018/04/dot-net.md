# .NET

## Microsoft API Compatibility Analyzers 

Microsoft released [an analyzer Nuget package](https://www.nuget.org/packages/Microsoft.DotNet.Analyzers.Compatibility/0.2.12-alpha) that you can use to ensure you have full compatibility for a cross platform target project or solution. And if you want to totally rely on Windows-only features (such as Active Directory or the registry), there is also the [Windows Compatibility Pack](https://blogs.msdn.microsoft.com/dotnet/2017/11/16/announcing-the-windows-compatibility-pack-for-net-core/).

## Nintendo Switch emulator in C#

Have a look at [this Github repository](https://github.com/gdkchan/Ryujinx) where the guys are building a Nintendo Switch emulator on .NET Core 2.0. This is pretty rad stuff. There is a prebuilt version too.

## CultureInfo and parsing string values

Model binding is a science all on it's own, and typically not something we mostly ever have to consider. MVC, WPF, the old SOAP references, Newtonsoft all employ some form of model binding. JSON format standards have made things considerably easier though, but every now and then you have to manually do something. Consider the following code snippet:

```
decimal value = -45789.12M;
value.ToString("N2", System.Globalization.CultureInfo.InvariantCulture.NumberFormat)
value.ToString("N2", System.Globalization.CultureInfo.CurrentCulture.NumberFormat)
value.ToString("N2", new System.Globalization.CultureInfo("en-ZA").NumberFormat)
value.ToString("N2", new System.Globalization.CultureInfo("en-UK").NumberFormat)
value.ToString("N2", new System.Globalization.CultureInfo("fr-FR").NumberFormat)
value.ToString("N2", new System.Globalization.CultureInfo("sl-SI").NumberFormat)

//results:
//-45,789.12
//-45 789.12
//-45 789.12
//-45,789.12
//-45 789,12
//-45.789,12
```
I constructed this in LINQPad, so replace `.Dump()` with `Console.Writeline()` or similar if you don't have that tool. Here it's clear that different cultures employ different formats, and when you export values (including `DateTime` values), you might not get the desired results. Also notice how `InvariantCulture` and `CurrentCulture` yields different results. Consider the case where your developer machine is configured with `English - South Africa`, but you deploy to a client's server in Germany which is setup differently (this _actually_ happened on the N-Gage/CrossCore project). Formats are all wonky, and unless you keep consistency by referencing `CultureInfo.InvariantCulture`, your results may vary.

Now Consider the following piece of code:

```
string representation = "-45.789,12";
decimal value = ??
```

Without knowing what culture that given number value is in, it becomes difficult to bind to a field or property. `CultureInfo.InvariantCulture` doesn't work here. There is only one option:

```
decimal value = decimal.Parse(
  representation, 
  new System.Globalization.NumberFormatInfo() { 
    NumberGroupSeparator = ".", 
    NumberDecimalSeparator = "," 
});
```

This is very explicit and will require too much pre-condition checking to build support for multiple cultures. So what do you do in an API environment where you want to support multiple cultures/localization in a generic way? Let your client tell you what culture is being used. [Read the `Accept-Language` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language) for a locale. This is the client's way to tell you want culture it accepts. If they don't send anything, assume `en-US` (equivalent of `InvariantCulture`), and then [use the `Content-Language` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Language) to tell the client in which locale the response is. If you follow this "handshake" with the client, then you can write code like:

```
string locale = request.Headers["Accept-Language"];
decimal value = decimal.Parse(
  representation, 
  new System.Globalization.CultureInfo(locale));
```

## March Code Challenge - Map the collection

I set this up to satisfy my own curiosity, and to also see what you might come up with. This was a problem I faced in TeamNinja: given a collection of contacts, generate a set of emails using a template with a unique access link for each contact, and send it all out. As part of this challenge I built seven (sufficiently) different solutions to benchmark. I also received two submissions.

### The setup

I constructed the problem across two scenarios:
1. Read the token file in once at start, and serving from memory, perform an actual SHA1 hash based on the token and reduce to ASCII [A-Za-z0-9] characters. This is a 100% CPU based workload for each contact.
2. Read the token file _for each_ iteration, and serve only the relevant index. No other work was done. This is a 90% IO based work load for each contact. IO is different from CPU work in that the [disk, network, keyboard etc] controller is mostly synchronous, with a very limit level of caching which results in a queue building up for a lot of read and write requests.

All scenarios were executed on a set of 10 000 pre-generated tokens.

### The metrics

I only tracked execution time (in milliseconds) for each implementation. However, I wanted to see how the different implementations behave in the two different scenarios (CPU vs IO). In the real world there would probably be some repository method call for contacts to retrieve persisted data, and although disk IO doesn't simulate SQL Server drivers (or Mongo drivers) exactly, I think it is relevant to inspect the behaviour should there be some other blocker or queue outside of your immediate algorithm's control.

### The implementations

The sourcecode can be viewed (and pulled) [here](http://dockerhost.armorica.gk/gitlab/gk/dotnet/cc/collectionmap), which details the list of implementations I benchmarked:
* ListForEach - A simple `.ToList().ForEach()`.
* ListForEachAsyncAwait - A simple `.ToList().ForEach(async)`.
* SelectToList - A `Select()` mapper, wrapped in an `await Task.FromResult()`. `Select()` does not allow `async` lambdas.
* SimpleForEach - A simple `foreach` with `async` and `await`.
* ParallelForAll - An `AsParallel().ForAll()` operating on a `List<>`.
* ParallelForAllThreadSafe - An `AsParallel().ForAll()` operating on a `ConcurrentBag<>`.
* ParallelForAllAtomic - An `AsParallel().ForAll()` operating on an `Array`, leveraging the atomic nature of integer operations.

### The results

The raw data was normalized due to the big difference between CPU and IO work loads.

|Implementation            | CPU     | IO       | CPU-n | IO-n |
|-------------------------|---------:|----------:|-------:|------:|
|ListForEach              |1911.6365|54622.4200|1.4077 |0.2356| 
|ListForEachAsyncAwait    |1508.8554|55949.7178|-0.2974|0.5170|
|SelectToList             |1491.9812|56736.9993|-0.3688|0.6839|
|SimpleForEachAsyncAwait  |1561.9575|59639.7858|-0.0726|1.2992|
|ParallelForAll           |1488.4120|50906.6241|-0.3839|-0.5521|
|ParallelForAllThreadSafe |**1347.5235**|**47271.9429**|-0.9803|-1.3226|
|ParallelForAllAtomic     |1386.7070|48062.7938|-0.8145|-1.1550|
|John                     |1474.0951|49381.9493|-0.4445|-0.8753|
|Bolanki                  |2040.7641|59027.0538|1.9543|1.1693|

<p align="center">
  <img src="dotnetbench.png" alt="Benchmark Results"/>
</p>

I found it particularly interesting that, apart from the `AsParallel` implementations, the performance on CPU load is the inverse of the performance on IO load. It is also remarkable that the simple `ToList().ForEach()` performs so much worse than the `ToList().ForEach(async)` on CPU load. The LINQ engine clearly understands the crucial difference between supplying an `async` lambda and not, and generates very different IL; Toptip: stay away from `.ToList().ForEach()` and rather use a `foreach`. But only for CPU load. When it comes to IO, an asynchronous implementation simply overloads the IO device driver, with all the threads being served one after another.

But then the `AsParallel().ForAll()` methods come and trounces everything, even on IO. Why this is, is beyond the scope of this newsletter (and me, for that matter), but there _are_ a few caveats to take note of here: While `async await` implementations preserve execution order and _only threads when there are no dependencies_, `AsParallel().ForAll()` does not perform these integrity checks. There is no guarantee as to the order of execution, and the result cannot be determined beforehand (meaning you cannot assert on a specific value on an index in a unit test). You are also required to manage any and all threadsafe-tiness between the items in an `AsParallel().ForAll()`. The first implementation uses a `List<>`, and the contents of that list does not constitute a correct result after running the benchmarks - it is incomplete and typically misses about 1% of entries as threads overwrite each other. The results for the threadsafe `ConcurrentBag<>` implementation is solid, but with non-deterministic order. Also note, the atomic implementation is an experiment and doesn't contribute here, but it is significant that even without any concurrency management and a pre-initialized array, it performed slower on both CPU and IO loads than with `ConcurrentBag<>` overhead.

### The conclusion

I know most of us don't have time to build and perform benchmarks for all the code we write. And there is the practice that we first have to write code that works, and then make it maintainable and elegant, and then only do we optimize. But there are significant differences between asynchronous behaviour vs true parallelism or concurrency which we should know about upfront. You also need to combine this with your use case and your requirements. In this specific use case (sending out emails to contacts) the order does not matter, as long as we process the correct data. In other use cases order is important.

Then there is the matter of your prefered style. For me personally the `Select()` mapper implementation is the best. It's a more functional approach with pretty much middle of the road performance on CPU load. It's just a pity that it doesn't support `async await`. Which pattern is your favourite?

### The submissions

It must be noted that this challenge was not a competition. I included both John and Chris' implementations in the source. In both cases I had to modify their implementations to match the provided pattern, and in both cases the provided solutions were overly complicated and contrived. John used a different pattern for the same concurrent implementation (`Parallel.ForEach()`), but also used a non-threadsafe `List<>` backing store. His execution time matched the basic `AsParallel().ForAll()` implementation, and his results was also wrong and missed about 1%. Chris's implementation used a basic `foreach` with `async await`, but his execution time is significantly slower on CPU load than the SimpleForEach implementation. I contribute this to his upfront validations ( `IEnumerable.Any()` and null checks), as well as his method calls which passes an array, the contact Id and an instance of `Contact` as parameters. This all adds overhead on the stack (and such method calls are not always in-lined in IL depending on circumstances). This is an example of a maintainable implementation (heavy on the single responsibility principle) over an optimized one, with a good measure of defensive coding.

