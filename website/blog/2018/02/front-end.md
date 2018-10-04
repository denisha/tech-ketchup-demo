# Front-end 2018

## Progressive(ly) taking over the world

_One device at a time_

This year (2018) brings great things for all front-end developers with much wider browser vendor
adoption for _Progressive Web Applications_ through
[service workers](https://developers.google.com/web/fundamentals/primers/service-workers/). We at
Global Kinetic have already embraced this enhancement by starting work our first _Progressive Web
Application_ for financial advisers. For some more information on the application, have a chat with
Chamir or Hendrik.

#### What is a _Progressive Web Application_?

> Progressive Web Apps are just great web sites that can behave like native apps—or, perhaps,
Progressive Web Apps are just great apps, powered by Web technologies and delivered with Web
infrastructure.
- _[Windows Blogs](https://blogs.windows.com/msedgedev/2018/02/06/welcoming-progressive-web-apps-edge-windows-10/)_

A web application that is installable to the home screen (The home screen of your mobile device, or
your desktop), which is available even when offline. Through
[service workers](https://developers.google.com/web/fundamentals/primers/service-workers/),
_Progressive Web Applications_ can send push notifications to the user's device directly, too.

#### Installable, you say?

Yes, it is true. When visiting a _Progressive Web Application_ on your mobile device, after it has
been cached you will receive a prompt to add it to your home screen. This installs the application
on your device, and it then functions like a native application, even opening in it's own window.

On desktop devices _Progressive Web Applications_ can also be installed, which will add a shortcut
on your desktop that will open the application in it's own window.

#### Offline availability

A _Progressive Web Application_ can cache all application files and data through the use of the
[Cache API](https://developers.google.com/web/fundamentals/instant-and-offline/web-storage/cache-api)
and [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API). The
[Cache API](https://developers.google.com/web/fundamentals/instant-and-offline/web-storage/cache-api)
can cache all assets (HTML, Javascript, CSS, Images, Fonts) and then use the cached versions instead
of re-fetching them from the server on each page load.
[IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) is a browser object
store that can store all data in plain objects. This data can then be used to sync when that
application comes online again.

#### What about updates?

Updates to _Progressive Web Applications_ are automatically downloaded in the background when any
change to the
[service worker](https://developers.google.com/web/fundamentals/primers/service-workers/) is
detected. The next time the application is opened the latest version will be used. No more app store
updates, just a seamless user experience.

#### How do I build my own?

The best place to start would be to follow along with a tutorial, the
[Google Developer article](https://developers.google.com/web/fundamentals/codelabs/your-first-pwapp/)
is full of information and takes you through building a real application from start to finish. Once
you have mastered the basics it is highly recommended to take the time and read through the
[Building Progressive Web Apps](https://www.safaribooksonline.com/library/view/building-progressive-web/9781491961643/ch01.html)
book by Tal Ater. This book is very in depth, and should give you a good understanding of the
ideology and mechanisms.

#### Browser support

Google Chrome and Firefox have supported _Progressive Web Applications_ for a while now. Microsoft
Edge has come to the party and have very recently announced service worker support in the Edge
preview builds. They have also said that they will soon start crawling the web and are planning to
add _Progressive Web Applications_ directly to the Windows 10 store.
[Source](https://blogs.windows.com/msedgedev/2018/02/06/welcoming-progressive-web-apps-edge-windows-10/).
Chromium has also announced a while ago that they are removing support for installed Chrome Apps
in favor of _Progressive Web Applications_.
[Source](https://blog.chromium.org/2016/08/from-chrome-apps-to-web.html).

## Security part one: NSP

With the immense amount of packages added to any modern front-end application, it is totally
impossible to be sure that none of them are vulnerable to outside attacks. Fortunately, the team at
[NodeSecurity](https://nodesecurity.io), and it's contributors, have spent a lot of time finding
and reporting vulnerabilities in our favourite npm packages.

#### From the command-line

Because we want to be consistently sure that packages we install are not vulnerable to attack, we
want the ability to add vulnerability checks to our _Continuous Integration_ checks/tests. For this,
the [NSP](https://www.npmjs.com/package/nsp) npm package is an amazing little tool, with it we can
add a check to our `npm test` command.

#### Open source integration

The [NodeSecurity](https://nodesecurity.io) application offers an easy to use GitHub integration.
Simply by signing up to their website and adding a repository to track, NSP will run as a pre-check
for each pull request in your open source application. This will help to stop you from merging in
unexpected surprises.
