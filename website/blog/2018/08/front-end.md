# Front-End

## A new contender to Node.js, almost

Although still a work in progress, [Deno](https://github.com/denoland/deno) is an up-and-coming
contender to [Node.js](https://nodejs.org/en/) created by
[Ryan Dahl](https://en.wikipedia.org/wiki/Ryan_Dahl). In his talk "10 Things I regret about Node.js"
he talks about what he doesn't like, and how Deno plans to solve these issues.

[10 Things I regret about Node.js (Video)](https://www.youtube.com/watch?v=M3BM9TB-8yA)

[10 Things I regret about Node.js (Text)](https://medium.com/@imior/10-things-i-regret-about-node-js-ryan-dahl-2ba71ff6b4dc)

## Clustering a Node.js application

[Node.js](https://nodejs.org/en/) applications are single-threaded, right? Yes, well, no, uhm,
sort of.

Although a single [Node.js](https://nodejs.org/en/) instance does run on a single thread, there is
a native way to take full advantage of all the CPU cores available. This is done through the use of
a [Cluster](https://nodejs.org/api/cluster.html).

A [Cluster](https://nodejs.org/api/cluster.html) creates child processes that can all share the
same port. Here is an example of a [Cluster](https://nodejs.org/api/cluster.html) script:

```js
const cluster = require('cluster');
const os = require('os');

const logger = require('./shared/logging');

if (cluster.isMaster) {
  for (let cpu = 0; cpu < os.cpus().length; cpu += 1) {
    cluster.fork();
  }
  Object.keys(cluster.workers).forEach(
    // eslint-disable-next-line no-console
    id => logger.info(`Worker created with ID \`${cluster.workers[id].process.pid}\``),
  );
  // eslint-disable-next-line no-console
  cluster.on('exit', worker => logger.warn(`Worker with ID \`${worker.process.pid}\` died`));
} else {
  // eslint-disable-next-line global-require
  require('./application');
}
```

This file is saved as `index.js`, where `application.js` can be assumed to be a web server of some
kind, [Express.js](https://expressjs.com/) for example.

Now it is possible to start the server through the standard `node` command as you usually would:

```sh
$ node index.js
```

This will output something along the lines of:

```sh
INFO sample-application: Worker created with ID `71`
INFO sample-application: Worker created with ID `72`
INFO sample-application: Worker created with ID `73`
INFO sample-application: Worker created with ID `74`
INFO sample-application: Sample Application listening on 0.0.0.0:3000
INFO sample-application: Sample Application listening on 0.0.0.0:3000
INFO sample-application: Sample Application listening on 0.0.0.0:3000
INFO sample-application: Sample Application listening on 0.0.0.0:3000
```

This still works just as well when running the application with a process manager such as
[nodemon](https://nodemon.io/).
