# Front-End

## Best friends forever

_BFF: Backend for Frontend_

In some projects here at GK we are utilising the power of the _BFF_. A _BFF_ is a facade layer for
easier orchestration of APIs and services.

**The problem** we are facing in many applications is that to display relevant information to a
frontend system there are cases where multiple APIs or services need to be called, to show a single
page to the end user. As an example, imagine we are building a page that shows a list of accounts,
filterable by account type. In this example the frontend application needs to fetch the list of
accounts as well as the list of account types. The frontend application then also needs to wait for
(show an indicator while) both these calls are in-flight. Now imagine we also want to filter by some
other criteria, which we also need to fetch a list of data for. Things get complicated.

**The solution** is the _BFF_. A small layer (written in [NodeJS](https://nodejs.org) in our case)
which sits between the frontend application and the APIs/Services that will handle the aggregation
for us. This layer is tightly-coupled to the frontend application. Now our application only needs to
make a call to a single endpoint, exposed by the _BFF_, and not need to do the aggregation and
orchestration of these calls itself.

The _BFF_ can also do proper data aggregation after a call/calls are completed, giving the
application only the data it is interested in, and only in the shape that it is interested in. The
_BFF_ is also a great place for security implementations that the frontend should not be worried
about, like proxies and certificates.

Another great benefit is that each call can be mocked/stubbed on the _BFF_ layer until the APIs for
it are ready, these mocks can also be used to write better unit tests for _BFF_ endpoints.

For a more in-depth explanation and some more use-cases there is a great
[Sam Newman article](https://samnewman.io/patterns/architectural/bff/) that explains the _BFF_
pattern in more detail.

## Up for a challenge?

If you want to prove your worth in the coding arena, head on over to this year's
[Entelect Challenge](https://challenge.entelect.co.za/) and build your own tower defense bot.
