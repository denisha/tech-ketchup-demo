# iOS

## `guard` vs `if let` cheat sheet (Swift 4)

### When to use `guard`

1. If you want to execute branching code when the _unwrapping fails_, rather than when it succeeds.
2. If you have to _halt execution_ in the current enclosing scope _if a value is incorrect or nil_.
3. When you need to know/want to log which optional _didn’t have the correct value_.
4. If there’s potential to _unwrap multiple optionals_ in the enclosing scope.
5. If there’s potential that the _number of optionals needed to be unwrapped can change_.
6. If you want the _resulting unwrapped value to be available for the remainder of the scope_ and not just within a branching scope.

### When to use `if let`

1. Want to execute branching code when the _unwrapping succeeds_, rather than when it fails.
2. When you can _continue execution_ in the current enclosing scope _if a value is incorrect or nil_.
3. _Don’t care which values are nil or incorrect_ within the enclosing scope.
4. If you only need to _unwrap a small number of optionals_ and that _number won’t change_ (avoid the pyramid of doom).
5. If you _only need the unwrapped value_ within a branching scope.

### *Bonus tip*

You can validate the resulting unwrapped optionals using multi-clause conditions.
<pre>guard let val = optional, val < 5 {} else {return}</pre>
<pre>if let val = optional, val < 5 {return} else {//Do something}</pre>

## A few tips to make you a better iOS (and overall) programmer 

### 1. Using _pure functions_
All functions should take all objects they need to do their work as arguments and return a value when done, rather than altering global state or creating other side effects.

##### This is a very, very basic (and almost non trivial) example of a function modifying the global state of a label:
<pre>
// defined somewhere else in your code
let usernameLabel = UILabel()
func updateUsername(_ username: String) {
    self.usernameLabel.text = username
}</pre>

##### The above example can be replace with the code below:
<pre>
// defined somewhere else in your code
let usernameLabel = UILabel()
func updateUsername(_ userlabel: UILabel, _ username: String ) -> UILabel {
    userlabel.text = username
    return userlabel
}</pre>

#### Advantages

1. Makes it easier to reason about your code as you have visbility of the objects they require and will modify.
2. Makes code easier to test, because eveything required for each function to run is passed in as an argument. You can now pass a mocked version of the object during tests.


### 2. Avoid using generic types to represent data
For us, this is most prevalent while parsing JSON data and being saved in dictionaries. Rather than relying on generic, built-in types, create custom types to represent your specific data. These can be simple wrappers around arrays and dictionaries with more descriptive naming and documentation, custom classes, or even enums to represent specific states rather than using Bools.

#### Advantages

1. Using custom types, especially with a strongly-typed language like Swift, means the compiler can help you avoid bugs and unexpected behaviour by ensuring you're always working with the exact type of values you expect. 
2. This will avoid coercion bugs when working with dynamic and loosely-typed data such as JSON (e.g. expecting an array but receiving a dictionary).
3. Makes your code more clear, as the shape of your values and objects is clearly defined and documented within your codebase.

### 3. Write code that explains itself
Make your code more expressive, rather than relying on comments to clarify functions, variable names, etc. Use those names to express clearly and succinctly what's happening, and save comments for explaining _how_ and _why_.

##### A simple example:
 
`let widthPx = 50`

instead of

`let width = 50 // width in pixels`

##### And a more complex one:

The first approach is safer, as it _checks_ assumptions, rather than _relying_ on those assumptions
<pre>// This is more clear and easy to read:
guard let height > 0 else { return 0 }
return width / height</pre>
<pre>// Compared to this, which requires a comment:
// Safe since height is always > 0
return width / height</pre>

#### Advantages

1. Relying on comments makes your codebase fragile for future colleagues or future you, since code can compile and run without comments being correct and up-to-date with the code.
2. This approach can help avoid regression bugs that come from misunderstanding code that's confusing, unclear or has comments that are incorrect or out-of-date.

### 4. Document functions and comment often
You can use the correct formatting for your comments that will make it parsable as function documentation (Xcode has keyboard shortcuts to help with this). You can then take advantage of XCode's documentation pop-ups for your own code. This is handy for quickly checking a function signature or reading notes about how a function works.

When you do need to add comments to your code, try to explain decisions you have made and any compromises you made that you might forget about in future. Also explaining any code that is non-trivial (algorithms etc).

#### Advantages

1. Documentation makes it fast and easy to use your own APIs and makes it obvious what to expect from those APIs. This makes working with your own code in future much easier and more enjoyable.
2. Comments can quickly clear up decisions and compromises that would be confusing or lead to future you/other developers making changes you've already decided against. 
3. Comments also makes it faster and easier to put code back into your head when you've been away a long time.
4. Comments can help when dealing with generic data (e.g. JSON from an API) that's not obvious from the types (e.g. basic, nested arrays and dictionaries vs. strict custom models and types).

### 5. Single-responsibility functions

Functions should do what they say they do—no more, no less. This means consumers of your APIs don't have to delve into the implementation of your functions to figure out what they do, so working with your codebase is more straightforward.

Types, classes, and functions shouldn't be responsible for anything you wouldn't guess, based on their name. This ties in well with the _pure functions_ concept, as a function without side-effects usually only does one thing anyway.

#### Advantages

1. When interacting with functions you shouldn't need to understand the internal implementation of those functions in order to use them.
2. Code with a single responsibility is more easily tested and refactored.

### 6. Composition
Using protocols to set up `has-a` relationships as opposed to the `is-a` relationships that come from inheritance enables composition—combining protocols to compose types that have particular traits. Where possible, protocol extensions can be used to create default implementations like a superclass would offer.

With composition, you can build and change your objects like LEGO, adding and removing functionality in a modular way.

##### Simple example: 

A `Bird` class that has a method `fly` and subclasses like `Finch` and `Magpie`. Now suppose you want to make a `Dragon` subclass. It needs to have a method `breatheFire` and it also needs to be able to fly... but it's not a `Bird`, so you don't really want to make it a subclass of `Bird` just to inherit the fly method.

If you make `fly` and `breatheFire` part of two different protocols, the `Dragon` class can conform to both in order to use those methods.

#### Advantages

1. Too many to mention. Have a look [here](https://medium.com/ios-seminar/protocol-oriented-programming-a-swift-solution-to-oop-headaches-e05d9c5d29a1) is a nice article with a basic example.


### 7. Dependency injection
Pass as arguments every dependency that an object requires when instantiating, rather than accessing global state or creating new objects from within the method. (There are other types of dependency injection, such as passing arguments into functions when they're called, or setting them as properties on an object or type after creating it).

1. Requiring dependencies as arguments makes your function definitions more clear, as it's obvious what objects and values are required for the function to do its work and where those are coming from.
2. Passing dependencies as arguments also helps a lot with the testability of your code, as you can easily switch out a depencdency with another when testing, passing mock objects and values as needed.


### 8. Plan new features end-to-end before coding
Think through as much of the implementation and the use-case of a new feature or a major refactor before coding. Write notes, draw diagrams, use flow charts; whatever works to help you figure out how your code will be implemented and how it will interact with the rest of your codebase.

#### Advantages

1. This process helps to save time and effort that comes from constantly backtracking when new features or refactoring don't work out. It helps you realise more quickly that a particular solution won't work so you can avoid wasting time implementing it.
2. Better planning can help you avoid making a mess of your codebase and/or commits by coming up with a solution that will work before coding, rather than constantly refactoring, resetting commits, or adjusting your approach partway through writing the code.

### 9. DRY / prioritise code reusability
**Don't Repeat Yourself**

Create reusable functions for any chunks of code you're using in different places, so the implementation itself isn't copied into various places in your codebase. Identify code that's logically the same but with minor changes around values, for example, and refactor these blocks out into a single function that takes the differing value as an argument.

DRY also works when defining values for things like UI. Develop a consistent UI style throughout the app by creating reusable theme elements (fonts, colours, etc.) and UI elements (pre-styled buttons, labels, etc.), defining these elements once and referencing them as needed. You can do this with a whole theming library or something as simple as a set of constants for your colour names and font sizes.

#### Advantages

1. Copy-pasta can lead to subtle bugs as well as code that's hard to read and reason about.
2. Copy-pasta-ed code also requires the developer to remember all the places the same code has been pasted when making changes in future, and to manually keep that code in-sync. This approach is extremely fragile and error-prone.
3. Makes code easier to test, debug and keep in your head, because you're only writing or changing code in one place.

### 10. Testing
Tests are designed to make sure your code behaves as you expect. They don't care about implementation, only outcomes.

<span style="color:red">Red</span>, <span style="color:green">green</span>, <span style="color:blue">refactor</span> is a _TDD_ (test-driven development) approach that starts with:

* writing tests that fail (because you haven't written/changed any code yet) [<span style="color:red">Red</span>]
* then writing code to make the tests pass, [<span style="color:green">Green</span>]
* then refactoring that code to make it more simple, robust, and/or elegant, [<span style="color:blue">Refactor</span>]

While relying on your tests to make sure you don't break what you just built. This approach ensures new features or changes come with tests, since the tests are written first.

#### Advantages

1. Tests can help you catch bugs or breaking changes as soon as you create them, which helps you ship better code. They also help you change your code with more confidence since tests will catch breaking changes or regressions as you work.
2. Writing tests in advance can also improve the usability and readability of your APIs, since you're planning with testability in mind.

### 11. Use assertions to ensure your code is being used correctly
Use assertions to define your expectations about arguments or conditions that should have been met, raising exceptions if they fail. Use them to test for things like making sure arguments are what you expect before trying to work with them or that a particular argument is present in cases where it's optional but you're expecting it to be there.

Use assertions as extra conditions on your arguments, when the type of object required in your definition doesn't ensure your code can run as expected. For example, your function takes an integer but you really need one that's not a negative and not zero.

#### Advantages

1. Assertions help to enforce correct use of your APIs and functions by yourself and other developers. You can avoid your users having a bad or broken experience by crashing in debug mode so you can find and fix programming errors before shipping.