# Front-End

## JSON Web Tokens

### What are they really?

A _JSON Web Token_ - or JWT - is a mechanism by which to represent claims in a URL-safe, structured
manner. This data can then be safely shared between two parties, in most cases these two parties are
a front-end client application and an API, but use-cases may vary.

### Representing claims?

Saying that a _JWT_ "represents claims" means that a single _JWT_ contains multiple pieces - or
claims - of information. These claims are wrapped up as key/value pairs of information in a JSON
object, thus the name _**JSON** Web Token_.

### Are JWTs secure?

A _JWT_ is signed using either a secret (read: password) or a public/pricate key pair. Although a
_JWT_ is signed, it's contents can still be read by any party. This means that although it can be
trusted to not have been tampered with (through signature verification), it's content is still
exposed to anyone who gains access.

Imagine a scenario where a _JWT_ is created by an API and then sent via a cookie to your client
application. The _JWT_ is signed on the API using a public/private key pair. Anyone using your
application can edit the cookie, which will be sent back to the API. The API can then verify that
the JWT is signed using the public/private key pair used to sign it in the first place, and can
reject any _JWT_ that has been tampered with.

This makes a _JWT_ safer than a plain string cookie, but still not completely secure.

### I want to know more

Don't we all?

If you want some more information - and some good examples - of JWTs, their shape, their contents,
typical use-cases and how they are built, head on over to
[https://jwt.io](https://jwt.io/introduction/) and have a read through their documentation, it's a
great place to really understand what makes up a _JWT_.

If reading is not your thing (I doubt it, you've made it this far already), have a chat with
Hendrik and he'll tell you more about where and how we are using _JWTs_ in some of our applications
already.
