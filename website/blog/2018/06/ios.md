# iOS

## What's new in Swift 4.2

### Derived collections of enum cases

In case we need to print all available enum values, we had to create some helper variable that includes all enum cases. For example, a static array called `allCases`. A big drawback in that approach is that we need to remember to update the `allCases` array every time when we modify enum cases.

Swift 4.1 approach:

```swift
enum CarType {
    case sedan
    case crossover
    case hothatch
    case muscle
    case miniVan

    static var allCases : = [.sedan, .crossover, .hothatch, .muscle, .miniVan]
}
```

in Swift 4.2 the new `CaseIterable` protocol will automatically generate an array property of all cases in an enum called `allCases`. These will be in the order you defined them in the enum.

```swift
enum CarType : CaseIterable {
    case sedan
    case crossover
    case hothatch
    case muscle
    case miniVan

    // there is no need to add allCases variable. CaseIterable protocol will synthesize it
}

for type in CarType.allCases {
    print(type)
}
```

**Important:** You need to add `CaseIterable` to the original declaration of your enum rather than an extension in order for the `allCases` array to be synthesized. This means you can’t use extensions to retroactively make existing enums conform to the protocol.


### Conditional Conformance

```swift
let arrayOfArrays = [[1,2],[3,4],[5,6]]
arrayOfArrays.contains([1,2]) // Swift 4.1: returns False
arrayOfArrays.contains([1,2]) // Swift 4.2: returns True
```
The conditional conformance works in the same way with `Hashable`, `Encodable` and `Decodable` protocols. It will work with `Optional` and `Dictionary` types as well
So for example, since `Int` is `Hashable`, it means that `Int?` and `[Int?]` will be `Hashable` as well.

```swift
let s: Set<[Int?]> = [[1, nil, 2], [3, 4], [5, nil, nil]]
s.contains([1,nil,2]) // returns true
```
### `Bool` toggle

`.toggle()` has been added to `Bool` which will toggle the value when called.

```
var isTheWeatherNice : Bool = true
print(isTheWeatherNice) // prints true
isTheWeatherNice.toggle() // In Swift4.2, this will change the bool value.
print(isTheWeatherNice) // prints false
```

Now we don't have to use an extension like this anymore:

```swift
extension Bool {
   mutating func toggle() {
       self = !self
   }
}
```

### `Hasher` struct

Swift 4.2 introduces the new `Hasher` struct that provides a randomly seeded, universal hash function to make conforming to the `Hashable` protocol process easier.

```swift
struct iPad: Hashable {
    var serialNumber: String
    var capacity: Int

    func hash(into hasher: inout Hasher) {
        hasher.combine(serialNumber)
    }
}
```
You can add more properties to your hash by calling `combine()` repeatedly, and the order in which you add properties affects the finished hash value.

You can also use `Hasher` as a standalone hash generator: just provide it with whatever values you want to hash, then call `finalize()` to generate the final value. For example:

```
let first = iPad(serialNumber: "12345", capacity: 256)
let second = iPad(serialNumber: "54321", capacity: 512)

var hasher = Hasher()
hasher.combine(first)
hasher.combine(second)
let hash = hasher.finalize()
```
`Hasher` uses a random seed every time it hashes an object, which means the hash value for any object is effectively guaranteed to be different between runs of your app. This in turn means that elements you add to a set or a dictionary are highly likely to have a different order each time you run your app.

### Random number generation

No more `arc4random`! :party:

You can generate random numbers by calling the `random()` method on whatever numeric type (`Int`, `Float`, `Double`, `CGFloat` and even `Bool`) you want, providing the range you want to work with. 

```swift
let randomInt = Int.random(in: 1..<5)
let randomFloat = Float.random(in: 1..<10)
let randomDouble = Double.random(in: 1...100)
let randomCGFloat = CGFloat.random(in: 1...1000)
let randomBool = Bool.random()
```
You can also get a random value from `Collection` types like `Array` or `Dictionary`.

```
let names = ["John", "Paul", "Peter", "Tim"]
names.randomElement()! 

let playerNumberToName : [Int: String] = [10: "Messi", 7: "Ronaldo"]
playerNumberToName.randomElement()! 
```
The `randomElement()` function returns an Optional, since a Collection can be empty.

```
let emptyCollection : [String] = []
emptyCollection.randomElement() // retuns nil
```

### Shuffling Collections
You can now shuffle arrays using new `shuffle()` and `shuffled()` methods depending on whether you want in-place shuffling or not.

```
var albums = ["Red", "1989", "Reputation"]
albums.shuffle() // shuffle in place
let shuffled = albums.shuffled() // get a shuffled array back
```

### Warning and error diagnostic directives (`#warning` & `#error`)
They introduced new compiler directives that help us mark issues in our code. These will be familiar to any developers who had used Objective-C previously, but as of Swift 4.2 we can enjoy them in Swift too.

The two new directives are:

1. `#warning`: Forces Xcode to issue a warning when building your code.
2. `#error`: Issues a compile error so your code won’t build at all. 

Both of these are useful for different reasons:

- `#warning` is mainly useful as a reminder to yourself or others that some work is incomplete. Xcode templates often use `#warning` to mark method stubs that you should replace with your own code.
- `#error` is mainly useful if you ship a library that requires other developers to provide some data. For example, an authentication key for a web API – you want users to include their own key, so using `#error` will force them to change that code before continuing.

Both of these work in the same way: `#warning("Some message")` and `#error("Some message")`.

```swift
func encrypt(_ string: String, with password: String) -> String {
    #warning("This is terrible method of encryption")
    return password + String(string.reversed()) + password
}

struct Configuration {
    var apiKey: String {
        #error("Please enter your API key below then delete this line.")
        return "Enter your key here"
    }
}    
```

Both `#warning` and `#error` work alongside the existing `#if` compiler directive, and will only be triggered if the condition being evaluated is true. For example:

```
#if os(macOS)
	#error("MyLibrary is not supported on macOS.")
#endif
```

### Updates to the `#if` diagnostic directive

Swift 4.1:

```
#if os(ios) || os(watchOS) || os(tvOS)
    #import UIKit
#endif
```
Swift 4.2:

```
#if canImport(UIKit)
```

```
#if hasTargetEnvironment(simulator)
```

### Condition matching on sequesnces
Swift 4.2 has a new method `allSatisfy()` that checks whether all items in a sequence pass a condition.

For example, if we had an array of exam results and we need to check if the student passed the course by achieving more than 85 or higher for all results.

```
let scores = [85, 88, 95, 92]
let passed = scores.allSatisfy { $0 >= 85 } // returns True
```

### In-place collection element removal

Swift 4.2 introduces a new `removeAll(where:)` method that performs a high-performance, in-place filter for collections. You give it a closure condition to run, and it will strip out all objects that match the condition.

For example, if you have a collection of names and want to remove people called “Terry”, you’d use this:

```
var pythons = ["John", "Michael", "Graham", "Terry", "Eric", "Terry"]
pythons.removeAll { $0.hasPrefix("Terry") }
print(pythons)
```
This will accomplish the same as:

```
pythons = pythons.filter { !$0.hasPrefix("Terry") }
```

But using `filter` is not very memory efficient when compared to the new `removeAll` method. More info about the implementation of `removeAll` can be read [here](https://www.dotconferences.com/2018/01/ben-cohen-extending-the-standard-library).