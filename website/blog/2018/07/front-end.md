# Front-End

## Interesting finds

A former developer at the _Google Photos_ team added an article to medium about the UI decisions
that went into creating the application all while keeping it fast and maintaining aspect-ratios
no matter how many photos you have. It's quite a long read, but has some really interesting
insights: https://medium.com/google-design/google-photos-45b714dfbed1

## NPM vulnerabilities

On July 12th two NPM packages were compromised which caused anyone who downloaded them to
unknowingly send their NPM access tokens to the attacker. This was caused by the developer re-using
the same password for all of their accounts. NPM published a postmortem of the incident a day or two
later which can be found on the
[eslint blog](https://eslint.org/blog/2018/07/postmortem-for-malicious-package-publishes).

If you publish packages to NPM you will need to create new access tokens, and as an added layer of
security it would be a good idea to change your password and enable
[two-factor-authentication](https://docs.npmjs.com/getting-started/using-two-factor-authentication).

## Two months left for NSP

On September 30th the NSP project is officially shutting down. You can read more about that on the
[NPM blog](https://blog.npmjs.org/post/175511531085/the-node-security-platform-service-is-shutting).
If - like me - you use this for a lot of your projects (all of the projects!) then it would be a
good time to start upgrading to `npm@6`.

Unfortunately there is still no _named_ `Dubnium` (Nodejs 10) Docker image (Source:
[Docker Hub](https://hub.docker.com/_/node/)), although there are some unnamed ones we might use in
the meantime, like `10-alpine`.

The LTS for that only starts the day after NSP shuts down (Source:
[NodeJS GitHub](https://github.com/nodejs/Release)). I mean, who wants a smooth
transition anyway?
