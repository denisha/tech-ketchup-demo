# .NET

## The State of .NET
Things in dotnetworld is in quite a turmoil at the moment. There is dissent among citizens about the state of Visual Studio 2017, thought-leaders are still posting and debatng the [differences between .NET Core and .NET Standard](https://msdn.microsoft.com/en-us/magazine/mt842506.aspx), and those using Framework are sitting on Windows version life-cycles while their Standard wielding contemporaries are looking at quarter sized life-cycles and weekly beta releases. What does that mean for you?

In the short term, not much.

![alt text](keep-calm-and-code-2808.jpg "Keep calm and code")

However, we now do have to make more informed decisions using longer term views about platform and architecture when we start a new project. There are times when we should use Framework, but mostly we _want_ to use .NET Core and .NET Standard. And there are differences in the APIs that you must be aware of in order to move between projects and platforms. Fortunately, Microsoft has (arguably) the best documentation available out there to aid you. So, if you thought that .NET development had become stale over the last 5 years between Framework 3.5 and 4.6, there's a whole new deep dive for you. It's easier than ever to build components in .NET Standard, for example, so take that github repository of yours and port it over now!

## Linting
Linting is a form of automated, template- or rule-based [static program analysis](https://en.wikipedia.org/wiki/Static_program_analysis) that aims to highlight possible bugs or bad code design or structure that could lead to bugs. Here is an [interesting narrative from John Carmack](https://www.gamasutra.com/view/news/128836/InDepth_Static_Code_Analysis.php) on this.

Some of you have been using resharper for years, and apart from the shortcuts it provides it also had a linting module that could be configured with a template and shared between members of a team. This was never a standard in any company I've been part of, and resharper has the unfortunate side-effect of killing performance. At GK, we're now using SonarQube. It is server-based (we run a [docker container](http://dockerhost.armorica.gk/sonar), so it will form part of your CI build but won't affect your Visual Studio instance. You can also install the [SonarQube extension](https://marketplace.visualstudio.com/items?itemName=SonarSource.SonarLintforVisualStudio2017).

## OSS
We started an exciting side-project for _everyone_ to participate in. The idea is that we will run a client-server turn-based board game that everyone can make a client for, in any technology. [More information here.](http://dockerhost.armorica.gk/gitlab/gk/dotnet/ttt/blob/develop/readme.md) This project (or projects) will not have a TFS board, stand-ups or even retrospectives. It will be run solely on the gitlab project issue tracker and wiki pages.

This is aimed at priming all of us for participation on the internet at large in other OSS projects, including .NET components we use every day and maybe even bits of the framework. It will also allow us to get familiar with typical OSS processes for when we release [our own components](http://dockerhost.armorica.gk/gitlab/gk/comp/dotnet) to github as OSS projects (think FutureBank clients).

## #noOO
Gary submitted a very very [interesting talk about how the Unity guys have ditched OO](https://www.youtube.com/watch?v=tGmnZdY5Y-E) in exchange for performance, and how they designed their new Jobs process to pander explicitly to the memory and processing requirements of their use case (games). This nicely illustrates different ways of thinking to solve different types of problems. If you are interested in using some of these techniques, come chat to me.