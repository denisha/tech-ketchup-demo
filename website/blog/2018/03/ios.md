# iOS

## GK's Carthage Expedition

We were trying to optimise the build times and ease of builds for the Old Mutual (OM) and Bidvest (BV) projects. 

The current build took about 30min just to get the latest code and compile the app (latest build took **28min 07sec**), add another few minutes for uploading the app to HockeyApp.

After our rework to make use of binaries (pre-compiled frameworks) the build time was to only **1min 40sec**.

Thats a reduction in build time of **94%** :tada:

<p align="center">
  <img src="mind-blown.jpg" alt="mind blown"/>
</p>

### Carthage - What they want you to know

Carthage is _"A simple, decentralized dependency manager for Cocoa"_ as per their definition, but like all (or most) iOS developers know it is never simple...

The reasons it appears *_'simple'_* is:
- It doesn’t modify your project files.
- There’s no central repository or library where developers need to submit their code to. (NPM, NuGet and Cocoapods)
- You are in full control of your dependencies and Carthage only fetches and builds them for use.

There are a few *drawbacks* for using Carthage:
- It only works with dynamic frameworks.
- There are few manual steps involved while adding and using Carthage in your project.
    - Create the dependency file (_Cartfile_).
    - Manually add the built framework files into your project.
    - Add 2 scripts to you build configuration.
- Its slooooooow, especially on CI Servers (in our case Jenkins).

The reason it is so slow is that Carthage checks out and builds each framework for every build. This even happens with local repositories, but unfortunately the majority of dependencies are hosted on Github. The dependencies does get cached, but if you want to make sure that that the latest code is always present, it needs to pull down and build the frameworks for every build. 

The speed issue was the main reason we had a look at reworking the implementation of Carthage on these projects.

### Our implementation

Our process enables the developers to continue to develop as they are currently with the safety of versioning of the sub frameworks. Currently the OM project shares about 75% (maybe more) of the code base with the BV project, which leads, or could lead, to unwanted and unnecessary code being present in the Bidvest project.

By using versioning, the developers can work with the release versions of the frameworks instead of the snapshot versions which will update multiple times per sprint, and also choose when to upgrade to the latest release version when the new features is needed.

#### Local vs external repositories 

The OM and BV repositories are hosted on our local _Gitlab_ server while the external dependencies, which there are quite a few of, is hosted on _Github_ externally.

While the building process using the local repositories are significantly faster than external repositories, it was still much slower than we wanted.

One feature that Carthage uses is downloading a pre-compiled binary of the framework if its available, instead of checking the repository out and building it. This is unfortunately only available on external _Github_ repositories that makes use of _Github_’s Releases feature. 

This would have helped a bit for the external dependencies if only they made use of this feature :triumph:. Needless to say, this won’t be any help with our local repositories.

Another feature we discovered hidden away at the end of a subsection in the _Carthage_ readme file, was using _Binary Only Frameworks_. 
In essence, this works on the same principle as the _Releases_ feature on _Github_, it’s a compiled file that is hosted somewhere which you can reference in your _Cartfile_ and pull down to use in your project. This negates the time wasted on fetching the code from the repository and building it completely, instead you download a small file that you can use immediately.

This obviously seemed too good to be true, but we started investigating it and came to the conclusion that it’s the only way for us to speed up both internal and external dependencies.

#### The wonder that is Binary Only Frameworks (BOF)

We soon realised it was a bit of extra work to get it working, but we were onto something. 

Seemed simple enough...

**Step 1 :** Create a json file in their specified format.
```
{
    "1.0.0": "https://linkToMyFile/1.0.0/framework.zip",
    "1.0.1": "https://linkToMyFile/1.0.1/framework.zip"
}
```
**Step 2 :** Host this file on a https enabled webserver.  
**Step 3 :** Make sure the link to the file is also https enabled.

Nowhere did they mention that the filename or structure of the _zip_ file. This gave me headaches for a few days until I stumble upon an an example of the structure hidden in a _Github_ repository.

After we figured that out, we started automating the process of making the BOFs available for us to use in our _Cartfile_. 

#### Our automated process (well, almost automated)

We are still refining our current process, but we have got it down to a stable working and pretty efficient state. 

The process's steps are listed below:
1. Build the framework [Include architectures for simulators and devices].
2. Compress the framework file.
3. Upload the zip to _Artifactory_.
4. Create or update the _json_ file used to store the references the various versions of the framework.
5. Commit this _json_ file to a repository to keep track of the changes of these files.
6. Host the contents of the repository on a webserver.

We have a few jobs on Jenkins that handle this process for us:

- **Steps 1-3** are all done in the ‘publish’ job. We have 2 variations for internal and external (_Github_) frameworks.
- **Step 4-5** are done in another job, which gets triggered when the _'publish'_ job is completed successfully.
- **Step 6** is done in the last job, which starts up a dockerised webserver that hosts the contents of the repository on our intranet.

When this is all done we can update the _Cartfile_ to reference the binary framework file (hosted locally), instead of the repository. This results in a small download of the _zip_ file instead of the timeous process of checking out the framework project via the repository and building it.


