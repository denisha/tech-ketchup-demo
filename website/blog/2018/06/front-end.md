# Front-End

## Building a Frontend Application

_The GK way_

Frontend applications have come a long way since the days of `HTML`, `CSS` and `jQuery`, here is a
look at how we here at **GK** build a Frontend application.

_Get a blankie and a cup of hot chocolate ready, this is going to be a long one._

### Containerization

Docker is love. Docker is life. All applications that we set up are now containerized to make sure
that the development and deployment processes are as smooth and painless as they can be. No more
worrying about which version of _NodeJS_ is installed, or that some framework/library is not working
on _Windows_.

To try and make this even smoother we always try and use the _Alpine_ versions for our base Docker
images which reduce size dramatically. _Alpine_ is a tiny linux distribution with only the base
operating system, and nothing more. A default _Alpine_ image with _NodeJS_ installed is **7mb** in
size, where the standard _NodeJS_ image is **135mb**, that's a lot of bandwidth saved.

### Framework

With so many Frontend frameworks available, it's really hard to choose the right one for the job.
There are many frameworks in the market, each with their own pros and cons, but for the sake of this
article we will be looking at the two top-dogs, [Angular](https://angular.io/) and
[React](https://reactjs.org/).

If you're looking to just install a framework and have everything required at your fingertips,
[Angular](https://angular.io/) (lead by the [Angular](https://angular.io/) team at _Google_) would
be the way to go. It's reach, together with it's almost-native integration of
[TypeScript](https://www.typescriptlang.org/) makes it a great entrypoint for developers from other
languages, and for large-scale projects.

If it's flexibility you're after, look no further than [React](https://reactjs.org/) (Maintained by
_Facebook_). A small framework with just the bare-bones components you would need to build a fast,
interactive user-interface. Everything else needed can be added on to a
[React](https://reactjs.org/) project with relative ease.

Because of the flexibility and raw size of [React](https://reactjs.org/), it is the go-to framework
for most projects here at **GK**.

### Testing

Because of the scale of the applications we build (size and impact), it is imperative that our code
is well tested. To make sure we try and cover all the gaps, three layers of testing are required,
_unit testing_, _linting_ and _vulnerability checking_, each of these performing a very specific
task.

#### Unit Testing

_Unit testing_ is the task of testing the smallest possible interfaces (not user-interfaces), each
in isolation through an automated testing suite. This ensures that the code that is written does
exactly what it is expected to do.

For [Angular](https://angular.io/) applications, the
[Karma](https://karma-runner.github.io/2.0/index.html) test runner is built-in along with it's own,
rather complicated [TestBed](https://angular.io/api/core/testing/TestBed). Although complicated
it comes with this pre-built, and you wouldn't need to go look for a _Unit testing_ framework out in
the wild.

[React](https://reactjs.org/) doesn't come with any _Unit testing_ framework, which means one will
have to be added. Even though this is the case, another _Facebook_ project -
[Jest](https://facebook.github.io/jest/) - makes _Unit testing_ as easy as it could possibly be.

#### Linting

_Linting_ is the use of automated tools to check code for "correctness" without running the actual
code. It is a set of rules that the code is checked against to make sure that all code produced is
readable, and contains no obvious issues.

[Angular](https://angular.io/) ships with [TSLint](https://palantir.github.io/tslint/), a _linting_
tool for [TypeScript](https://www.typescriptlang.org/).

[React](https://reactjs.org/) does not ship with any _linting_ tools, but
[ESLint](https://eslint.org/) is incredibly easy to add to any project.

#### Vulnerability Checking

Because of the abstracted nature of all modern Frontend applications (by using heaps of
inter-dependent small [npm](https://npmjs.com/) packages), there is a large possibility that unknown
bugs or vulnerabilities could sneak in to our code (Like
[harvesting credit card numbers](https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5)
from applications). To try and mitigate this risk we implement
[NSP](https://github.com/nodesecurity/nsp) (remember the February newsletter Frontend section) which
scans through installed packages looking for vulnerabilities. Although it looks like
[NSP](https://github.com/nodesecurity/nsp) has been archived, it was actually bought by
[npm](https://npmjs.com/) and will be included in the next LTS release. This tool is
framework-independent.

### Logging

With the high amount of traffic between Frontend applications and the APIs they consume, it is
important to have a way to be able to write every transaction to a log that can later be used for
debugging/auditing.

Because Frontend applications cannot write directly to disk (they run on the end-user's device),
a middle layer, like the _BFF_, (remember the April newsletter Frontend section) comes in handy.

The _BFF_, together with a logging library like [Winston](https://github.com/winstonjs/winston) or
[Bunyan](https://github.com/trentm/node-bunyan) can save numerous debugging hours when trying to
find out what is really going on.

### Building/Bundling/Packaging

To work properly on production systems, a Frontend application needs to be bundled into a set
of javascript files (and some assets) and hosted on web server software like
[NginX](https://www.nginx.com/) or [Apache](https://httpd.apache.org/) to allow proper speed and
scaling capabilities.

[Angular](https://angular.io/) comes built-in with [Webpack](https://webpack.js.org/) bundling
capabilities through their `ng` command-line tool.

[React](https://reactjs.org/) does not ship with any bundling tool and one needs to be added. There
are quite a few bundlers available, with [Webpack](https://webpack.js.org/) being the most popular
by far.

## Deployment

While developing, the application needs to be deployed to a number of environments for testing,
signoff, and production purposes. This is all done through the use of a deployment server like
[Jenkins](https://jenkins.io/). We use pipeline scripts in our source-control to automatically
built, test, and deploy our applications through all environments.

Each _Merge Request_ will run the suite of automation tests (_Unit testing_, _Linting_ and
_Vulnerability checking_) as well as a dummy _Build_ before starting the deployment process
through all the environments.

The deployment script will merge the code from the _Merge Request_ in, run the test suite again,
create a build artifact, and then promote the artifact through all the required environments.

## Analytics

After deploying the application it becomes necessary to analyze it to watch for any anomalies.
Analytics can be broken up into two sections, _Usage analytics_ and _Code analytics_.

### Usage Analytics

Although we think we know, we actually have no idea how our end-users use the application(s) we
build. To see how it is really used, a tool like [Google Analytics](https://analytics.google.com)
which shows a whole array of useful information about how our application is being used in the real
world.

### Code Analytics

Apart from tracking user behaviour, we also need to track how code is growing as we add more
features release by release. To analyze how our code is performing (in terms of
bugs/coverage/issue/technical debt) we use [SonarQube](https://www.sonarqube.org/) which is updated
every time we merge in some code.

We have a few projects on our [SonarQube](https://www.sonarqube.org/) server already, and are adding
more as we get some spare time.

### Monitoring

To make sure our application(s) don't fall over during the night we make use of monitoring software
to do health checks on out them at regular intervals. This software will inform us (via email or
message) when a service falls on it's face. We use [Nagios](https://www.nagios.org/) to keep track
of our applications and keep us informed.

Hopefully the dungeon will soon be the home of a new monitoring monitor on one of the walls where
this will be visible so we can have even more eyes on it.

## Conclusion

Frontend applications (and the tools around them) have come a very long way in the last few years,
we here at **GK** are constantly striving to be the best and use the best tools available to do our
jobs properly and securely.

If you have any questions around the process(es) above, or would like to see any of them in action
please have a chat with Hendrik.

Well done, you made it.
