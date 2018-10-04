# Hack yourself first
[This site](https://hackyourselffirst.troyhunt.com/), setup by security personality Troy Hunt, was built specifically to help you identifiy security holes on web content. It is accompanied by his [Pluralsight course](https://www.pluralsight.com/courses/hack-yourself-first) of the same name. Use it to learn what not to do when you make web pages.

# Taking, storing and working with passwords

[This blog post](https://www.troyhunt.com/passwords-evolved-authentication-guidance-for-the-modern-era/) by the same guy (he's really great!) links to and points out the main guidelines (from standards authorities and Microsoft) when you are building a system and need to take in, store and handle users' passwords. This is as much a user journey in confidence in your system as it is a critical feature to get right. TL;DR:
* Don't limit or restrict password length or possible characters.
* Don't build subsystems like password hints.
* Don't break password managers (e.g. allow pasting of passwords into your app).

Please read it since it contains a lot of other important and rather surprizing pieces of information.