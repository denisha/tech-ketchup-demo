# .NET

## Bing updated to run on .NET Core 2.1

Microsoft is following Stack Overlow in [switching over to .NET Core 2.1](https://blogs.msdn.microsoft.com/dotnet/2018/08/20/bing-com-runs-on-net-core-2-1/) for one of their biggest products.

## Basic decency for programming

[This post](https://www.stilldrinking.org/how-to-worry-less-about-being-a-bad-programmer) about not feeling insecure about finding answers on the internet is truth. Don't feel bad. I'm the first one to proclaim that I can't code when Stack Overflow becomes unreachable. And it's not even a joke. However, there _are_ a few things that you need to keep in mind.

### Plagarism is bad!

* **If you copy, don't copy verbatim.** This applies not just to essays but also to code. If all you do is copy and paste then you _should_ feel bad indeed. You're not learning anything, and you're doing your codebase a disservice. Instead understand why it is the answer that you're looking for, and try and figure out how you could have reached that conclusion yourself. Go look through the namespaces in the framework for links to the classes, APIs or extensions used in the answers. Dive into the .NET code. In your unit tests you will probably have to mock some of the stuff anyway, and then it will be useful to understand.
* **Instead of pasting, rewrite.** I've seen code pasted into the codebase directly from SO that declared variables, but weren't being used in the codebase. The copy-paster was interested in a certain section of the answer, but instead copied the entire answer and simply changed what was required. The rest of the answer code was left there to rot. It is so blatantly obvious, creates a smell (literally SonarQube will report a code smell), and generates warnings in the IDE also.
* **If you use an answer or blog post, credit the author in code.** Sometimes the answer is non-trivial. Sometimes the answer is an entire implementation of a small service (for example a collection of extension methods). Always paste a code comment with the URL of the question/answer/blog page in the code. This is so that other developers (or future you even!) can follow back to that answer to re-read why it was the answer. It might also be that the answer has been updated, or a second or better answer had been supplied after an update to the framework. A good example is how authentication and authorization had changed from .NET Core 1.1 to .NET Core 2.0. Similarly, how xUnit had changed from those two framework versions.

<p align="center">
  <img src="SO_Copycat.jpg" alt="Stackoverflow Copycat"/>
</p>

## Trouble with the BOM

Consider the following piece of code:

```c
string result = Encoding.UTF8.GetString(ms.GetBuffer());
```

It's very simple and easy, and we've all used it before. However, if `result` is supposed to be some XML, and you try to stick it into an `XmlDocument` object, you get an exception detailing that there is an "illegal character on line 1, position 1". What? There's nothing showing in the debugger except for `<`?!

It turns out that the static short-cut property `Encoding.UTF8` instantiates and returns the default implementation of the `UTF8Encoding` class:

```c
UTF8Encoding encoding = new UTF8Encoding();
```

This default constructor initializes the encoding object in such a way that it creates a BOM (byte order marker) at the start of the string. This is an UTF8 identifing character, and it's also a non-printable character. You won't see it in the debugger or in notepad. No problem, you've never had to deal with it before. Until you've tried to load the XML that comes after it.

So, you can remove it retrospectively:

```c
string _byteOrderMarkUtf8 = Encoding.UTF8.GetString(Encoding.UTF8.GetPreamble());
if (result.StartsWith(_byteOrderMarkUtf8))
{
    result = result.Remove(0, _byteOrderMarkUtf8.Length);
}
```

Or, you can just read the documentation properly:

```c
public class UTF8Encoding : Encoding
{
    public UTF8Encoding();
    public UTF8Encoding(bool encoderShouldEmitUTF8Identifier);
    public UTF8Encoding(bool encoderShouldEmitUTF8Identifier, bool throwOnInvalidBytes);
    ...
}
```

There is a constructor here to specifically turn that bit of the encoder off. So this is an example of looking at and simply copying from Stack Overflow. And this was your tech lead's copy-paste effort. Instead of understanding where the problem comes from, I copied the code to remove it afterward, adding multiple instructions that will cost a few milliseconds each time the routine is called. Had I done a deep dive into the .NET framework and investigated a bit futher, e.g. [another SO post](https://stackoverflow.com/questions/11701341/encoding-utf8-getstring-doesnt-take-into-account-the-preamble-bom), I could have _written_ much better and cleaner code, instead of _pasting_ code that declares an additional variable and introduces another branch in the method.