# iOS

## What’s new in Swift 5.0

Swift 5.0 is the next major release of Swift, and is slated to bring ABI stability at long last. That's not all, though: several key new features are already implemented, including raw strings, future enum cases, checking for integer multiples and more.

### Raw strings
The ability to create raw strings, where backslashes and quote marks are interpreted as those literal symbols rather than escape characters or string terminators. This makes a number of use cases more easy, but regular expressions in particular will benefit.

To use raw strings, place one or more # symbols before your strings, like this:

``` swift
let rain = #"The "rain" in "Spain" falls mainly on the Spaniards."#
```

The **#** symbols at the start and end of the string become part of the string delimiter, so Swift understands that the standalone quote marks around "rain" and "Spain" should be treated as literal quote marks rather than ending the string.

Raw strings are fully compatible with Swift’s multi-line string system – just use `#"""` to start, then `"""#` to end, like this:

``` swift
let multiline = #"""
The answer to life,
the universe,
and everything is \#(answer).
"""#
```
### Handling future enum cases

The ability to distinguish between enums that are fixed and enums that might change in the future.

One of Swift’s security features is that it requires all switch statements to be exhaustive – that they must cover all cases. While this works well from a safety perspective, it causes compatibility issues when new cases are added in the future: a system framework might send something different that you hadn’t catered for, or code you rely on might add a new case and cause your compile to break because your switch is no longer exhaustive.

With the `@unknown` attribute we can now distinguish between two subtly different scenarios: "This default case should be run for all other cases because I don’t want to handle them individually," and "I want to handle all cases individually, but if anything comes up in the future use this rather than causing an error."

Here’s an example enum:

``` swift
enum PasswordError: Error {
    case short
    case obvious
    case simple
}
```

We could write code to handle each of those cases using a `switch` block:

``` swift
func showOld(error: PasswordError) {
    switch error {
    case .short:
        print("Your password was too short.")
    case .obvious:
        print("Your password was too obvious.")
    default:
        print("Your password was too simple.")
    }
}
```

That uses two explicit cases for short and obvious passwords, but bundles the third case into a `default` block.

Now, if in the future we added a new case to the enum called **old**, for passwords that had been used previously, our `default` case would automatically be called even though its message doesn’t really make sense – the password might not be too simple.

Swift can’t warn us about this code because it’s technically correct (the best kind of correct), so this mistake would easily be missed. Fortunately, the new `@unknown` attribute fixes it perfectly – it can be used only on the `default` case, and is designed to be run when new cases come along in the future.

For example:

``` swift
func showNew(error: PasswordError) {
    switch error {
    case .short:
        print("Your password was too short.")
    case .obvious:
        print("Your password was too obvious.")
    @unknown default:
        print("Your password wasn't suitable.")
    }
}
```

That code will now issue warnings because the `switch` block is no longer exhaustive – Swift wants us to handle each case explicitly. Helpfully this is only a *warning*, which is what makes this attribute so useful: if a framework adds a new case in the future you’ll be warned about it, but it won’t break your source code.

### Checking for integer multiples

Adds an `isMultiple(of:)` method to integers, allowing us to check whether one number is a multiple of another in a much clearer way than using the division remainder operation, `%`.

For example:

``` swift
let rowNumber = 4

if rowNumber.isMultiple(of: 2) {
    print("Even")
} else {
    print("Odd")
}
```

Yes, we could write the same check using `if rowNumber % 2 == 0` but you have to admit that’s less clear – having `isMultiple(of:)` as a method means it can be listed in code completion options in Xcode, which aids discoverability.

### Counting matching items in a sequence

Introduces a new `count(where:)` method that performs the equivalent of a `filter()` and count in a single pass. This saves the creation of a new array that gets immediately discarded, and provides a clear and concise solution to a common problem.

This example creates an array of test results, and counts how many are greater or equal to 85:

``` swift
let scores = [100, 80, 85]
let passCount = scores.count { $0 >= 85 }
```

And this counts how many names in an array start with "Terry":

``` swift
let pythons = ["Eric Idle", "Graham Chapman", "John Cleese", "Michael Palin", "Terry Gilliam", "Terry Jones"]
let terryCount = pythons.count { $0.hasPrefix("Terry") }
```

This method is available to all types that conform to `Sequence`, so you can use it on sets and dictionaries too.

### Transforming and unwrapping dictionary values with compactMapValues()

Adds a new `compactMapValues()` method to dictionaries, bringing together the `compactMap()` functionality from arrays ("transform my values, unwrap the results, then discard anything that’s nil") with the `mapValues()` method from dictionaries ("leave my keys intact but transform my values").

As an example, here’s a dictionary of people in a race, along with the times they took to finish in seconds. One person did not finish, marked as "DNF":

``` swift
let times = [
    "Hudson": "38",
    "Clarke": "42",
    "Robinson": "35",
    "Hartis": "DNF"
]
```

We can use `compactMapValues()` to create a new dictionary with names and times as an integer, with the one DNF person removed:

``` swift
let finishers1 = times.compactMapValues { Int($0) }
```

Alternatively, you could just pass the `Int` initializer directly to `compactMapValues()`, like this:

``` swift
let finishers2 = times.compactMapValues(Int.init)
```

You can also use `compactMapValues()` to unwrap optionals and discard nil values without performing any sort of transformation, like this:

``` swift
let people = [
    "Paul": 38,
    "Sophie": 8,
    "Charlotte": 5,
    "William": nil
]

let knownAges = people.compactMapValues { $0 }
```

### More still to come!

These are the features that have already been implemented in the Swift 5.0 branch, but more will undoubtedly come in the months leading up to Swift 5.0's release.

[ABI stability](https://swift.org/abi-stability/) (enables binary compatibility between applications and libraries compiled with different versions of Swift) remains the single largest feature for 5.0: many large companies (including almost certainly Apple itself) are waiting for this to happen so the great Swift migration can begin.






