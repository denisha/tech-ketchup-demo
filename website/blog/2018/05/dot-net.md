# .NET

## xUnit updates, drops CLI support

The xUnit Twitter account [announced the new release](https://twitter.com/xunit/status/1001321969189437440), and also dropped support for the `dotnet xunit` CLI tool reference/command. The release notes are [here](https://xunit.github.io/releases/2.4-beta2). As of the time of writing, an impact analysis on GK processes has not been concluded (but don't update and you'll be fine). However, Mark Isaacs has had success in using `dotnet test` calls on a Linux docker image, submitting to SonarQube and generating code coverage results, so things are not looking as bleak.

## .NET Core 2.1 is available

Microsoft recently announced that [.NET Core 2.1 is now officially available](https://twitter.com/dotnet/status/1001877224096649216). You are more than welcome to start migrating your nuget packages to the new version.

## Microhub, Gitsoft, etc..

Yes, all your source code are belong to Microsoft now. But in all fairness, it's a logical move for the Redmond company since they are now considered [the single biggest contributor](https://www.networkworld.com/article/3120774/open-source-tools/microsoft-s-the-top-open-source-contributor-on-github.html) to open source at the moment ([Although there are different ways of looking at it](https://medium.freecodecamp.org/the-top-contributors-to-github-2017-be98ab854e87)). A few tweets (and threads) from the various discussions I saw around this:

https://twitter.com/rob_pike/status/1003751344124059655

https://twitter.com/buritica/status/1003636470450671617

https://twitter.com/shanselman/status/1003730372163719170

https://twitter.com/shanselman/status/1003724309318250496

And [the blog post](https://blogs.microsoft.com/blog/2018/06/04/microsoft-github-empowering-developers/) by Microsoft CEO Satya Nadella.