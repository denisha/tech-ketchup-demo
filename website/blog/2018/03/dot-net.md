# .NET

## ErgoSense is starting

This is a new project that will kick off soon. It is a start-up centered around the ergonomic state of an office and features seat, desk and room sensors that form an IoT network reporting on the quality and occupation rates of the office space that you might be sitting in. We welcome Mark Stares back into the .NET world for this project!

## FutureBank is live

Many congratulations to the entire FutureBank team on their go-live with Bidvest. This is our second .NET Core/.NET Standard live release, and our first containerized micro-services architecture to see the light of day. If you have any questions, feel free to contact or walk over to Renier in the Pavi-Lion office.

## MasterMaths is rolling out

MasterMaths recently went live, and are in the process of rolling out and setting up 160 distance learning centres. This is a massive leap for MasterMaths as a business, and it is solely consulted to, guided and supported by GK and that team. This is an example of a project that has grown old (and in some ways outdated) before it even went live. To that effect there is now some really significant architectural updates slated for that project on the long-term roadmap, which has me really excited. Good devops will also play a significant role in mitigating problems during such phases of a project, and it will surely illustrate the critical role it has in quality software delivery, and the importance we now place on it.

## .NET Core 2.0 proved to outperform on AWS Lambda

A recent blog [details some benchmarks](https://read.acloud.guru/comparing-aws-lambda-performance-of-node-js-python-java-c-and-go-29c1163c2581) between various compiled and dynamic frameworks proving that 

>.Net Core 2.0 significantly outperforms 

While this is debatable (and it was indeed debated) there's no doubt that the framework has some serious punch compared to alternatives. The idea here is that you should use the right tool (read language) for the job. For example, if inconsistent access but quick response times is a key non-functional requirement, use Python or JS since the cold start times are significantly better than for C# or Java. It is all relative to your user stories, budget, client requirements etc.

## Square peg, round hole

In a feat of some hackiness, Scott Hanselman managed to get an [ASP.NET Core website running on the cheapest GoDaddy shared-hosting tier](https://www.hanselman.com/blog/RunningASPNETCoreOnGoDaddysCheapestSharedLinuxHostingDontTryThisAtHome.aspx). It's an interesting read, and he links to lots of new stuff under the hood of .NET Core 2.1. Some really neat hoop jumping here. This sort of thing clearly illustrates the **current trend towards cross skilling**.

## Vulnerability related to Kestrel

Microsoft published a [security advisory](https://github.com/aspnet/Home/issues/2954) related to the Kestrel web server underpinning much of ASP.NET Core. Essentially, you are affected if you have

>Any ASP.NET Core hosted application which is directly exposed to the internet, or hosted behind a proxy which does not validate or restrict host headers to known good values.

There are _lots_ of mitigation options available, including a provided piece of middleware that you can include in your configuration.

## Code Exercise

Consider the following definitions:

```c#
public class Contact 
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get;set; }
    public string Email { get; set; }
}

public class MessageData {
    public string RecipientName { get; set; }
    public string RecipientAddress { get; set; }
    public string ActionUri { get; set; }
}

public class UserService
{
    public async Task<IEnumerable<MessageData>> GetContactEmailParamsAsync(IEnumerable<Contact> collection) 
    {
        //your code here
    }

    protected virtual async Task<string> GetActionUriAsync(int contactId) 
    {
        return $"https://www.test.com/activate/{ (await System.IO.File.ReadAllLinesAsync("tokens.txt"))[contactId] }";
    }
}
```

Dump the following content (or similar) into a file in the root of your project/solution matching the name in the `GetActionUriAsync` method (if it's not clear, line numbers match contact Ids, and you can have as many as you want). This file and the method implementation is simply to simulate where some hashing or other stuff will occur in real life:
```
0590069077
8290641333
5700905112
5116524143
6384458097
1525208013
4163775298
4496355468
4117808245
5723401651
```

Complete the `GetContactEmailParamsAsync`  method implementation to transform a list of contacts into a list of email template parameter sets. Each set must address the contact in full, be sent to the correct address, and provide a unique tokenized link for each contact (obtained by calling `GetActionUriAsync`).

For completeness sake, the email template is represented by something similar to this:
```
Dear { recipientName }

Please follow the link below to activate your account.

<div style="button">
    <a href="{ actionUri }">Link</a>
</div>

Rgds
tech ketchup
```

In your implementation, consider the following:
* Speed of processing
* Elegance and 
* Maintainability and readability

If you want to submit an implementation, please send it to techketchup@globalkinetic.com