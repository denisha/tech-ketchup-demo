# .NET

## .NET Core 2.0 EOL in September

Yep, there is no LT support for 2.0. The 2.0 runtimes will become unsupported in September, so upgrade to 2.1 before then.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">.NET Core 2.0 End of Life Extended to October 1, 2018 <a href="https://t.co/KQAlfqjfGv">https://t.co/KQAlfqjfGv</a></p>&mdash; .NET Team (@dotnet) <a href="https://twitter.com/dotnet/status/1010269960948465665?ref_src=twsrc%5Etfw">June 22, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Here is the [2.0 to 2.1 upgrade guide](https://docs.microsoft.com/en-us/aspnet/core/migration/20_21?view=aspnetcore-2.1).

## What goes into .NET Standard?

James Newton King mused about whether [Newtonsoft.JSON should become part](https://github.com/dotnet/standard/issues/834) of the .NET Standard APIs. The response was [unequivical (with good links)](https://github.com/dotnet/standard/blob/master/docs/faq.md#why-is-jsonnet-not-part-of-net-standard) on github, but mixed on twitter:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Here is a hand grenade for you: <a href="https://t.co/mKJ5sFlJao">https://t.co/mKJ5sFlJao</a> 🙃</p>&mdash; James Newton-King ♔ (@JamesNK) <a href="https://twitter.com/JamesNK/status/1018989458618662912?ref_src=twsrc%5Etfw">July 16, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

It gives a lot of insight as to the role of the newly established [.NET Standard governance model and review board](https://github.com/dotnet/standard/tree/master/docs/governance).

## The next C# language evolution (it's mega!)

The latest C# language iteration saw the introduction of `Span<T>` and `Memory<T>`. They were necessary to take .NET Core 2.1 to the next performent level [in how it deals with the stack and memory allocation](https://msdn.microsoft.com/en-us/magazine/mt814808.aspx) (arrays, lists etc). This makes these constructs interesting and useful for algorythmic implementations (like computational modeling), but of limited use to us who are building glorified data pumps™ (aka enterprise solutions).

### Extension Everything

However, [a spoiler on the next language iteration](https://github.com/dotnet/csharplang/issues/1711) dropped this week in another github post. While I'm not going to repeat the contents here, I'm still super excited about the design possibilities these changes will bring for us, including how we design with OO, functional or extension based implementations for the dependency injection and other patterns.

## Async in depth

We've discussed async in the .NET forum before, and while we have limited time in that forum for deep dives, you can follow on from that in [this post](https://docs.microsoft.com/en-us/dotnet/standard/async-in-depth). It dispels some myths that some of us (including me) have held until now:

>Throughout this entire process, a key takeaway is that no thread is dedicated to running the task.

But this only applies to IO (networking, HDD, etc) or similar. For CPU-bound work it's different:

>CPU-bound async code is a bit different than I/O-bound async code. Because the work is done on the CPU, there's no way to get around dedicating a thread to the computation.

So even though this stuff is so heavily abstracted by the framework and the OS, the differences in what `async await` means in different contexts are really important!

### So what is the difference?

Interrupts! Without going into the detail of physical computer architecture, just know that anything not directly connected to the CPU, its cache and RAM (e.g it lives on the other side of the PCi or eSATA busses and typically involves a device driver) has no way to let anything else know when it has done it's work, except through issuing an interrupt for the OS to pickup.