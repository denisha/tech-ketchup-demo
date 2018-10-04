# iOS

## Apple news for July

* Starting July 2018, all iOS app updates submitted to the App Store must be built with the iOS 11 SDK and must support the Super Retina display of iPhone X.

* Apple shipped 3.5 million watches in Q2 2018 which was 30% more than last year.

* The iPhone 8 Plus was the top selling iPhone in the US during Q2 2018 at 24% while the iPhone X at 17% and the iPhone 8 at only 13%.

* Apple released Public Beta 3 of iOS 12 as well as PB3 of macOS Mojave.

* Adobe announced that they will launch a full version of Photoshop for iPad some time in 2019.

* Apple Music surpassed Spotify’s US Subscriber count for the first time at the start of July.

* 2018 MacBook Pro
	* The 2018 15" MacBook Pro with the new Core i9 chip is was being severely throttled due to thermal issues, but Apple has released a software 'fix' to remedy the issue.
	* Apple confirms 2018 MacBook Pro keyboard has a 'membrane' to 'prevent debris from entering the butterfly mechanism'.
	* Geekbench 4 scores indicate the 2018 15" models have a 12-15% increase in single-core performance, while multi-core performance is up 39-46%, compared to the equivalent 2017 models.


## Rewriting basic loops in a Fun(ctional) way

**Why do this?**

It makes code easier to read, understand and safer to execute.

To write proper Functional code we need to make sure there are *no mutable states* (no class instances nor any variables) and it should contain *no loops*.


### Filtering

A simple example of filtering is determining which elements has a `String` length of `6`.

```
let beers = ["Castle", "Heineken", "Amstel", "Hansa"]
```

**Old way:**

```
var result: [String] = []
for beer in beers {
    if beer.count == 6 {
        result.append(beer)
    }
}
```
**Fun way** [using `filter`]

```
let result = beers.filter { $0.count == 6 }
```
The content of `result` will be `["Castle", "Amstel"]` for both cases.

The `$0` in the closure represents the `n-th` element in the beer array. Thus the compiler is actually doing the following on all the elements:

```
"Castle".count == 6		// true
"Heineken".count == 6	// false
"Amstel".count == 6		// true
"Hansa".count == 6		// false
```

For each value that passes the condition, the `n-th` element is added to the `result` array, thus the final array then contains `["Castle", "Amstel"]`.

### Mapping

A simple way of mapping is to create an array of the same values of the properties of a `struct`. (Array of people's names)


Say we have a `struct` representing a Person

```
struct Person {
    let name: String
    let surname: String
    let birthYear: Int
}
```
We can create an array of people

```
let people = [
    Person(name: "Walter", surname: "White", birthYear: 1959),
    Person(name: "Jesse", surname: "Pinkman", birthYear: 1984),
    Person(name: "Skyler", surname: "White", birthYear: 1970)
]
```

**Old way**

```
var names = [String]()
for person in people {
    names.append(person.name)
}
```
**Fun way** [Using `map`]

```
let names = people.map { $0.name }
```

The content of `result` will be `["Walter", "Jesse", "Skyler"]` for both cases.


The `map` method allows you to transform a sequence into another one. Here we are mapping the `name` property on the `n-th` element in the `people` array. This results in the `names` array containing all the name values of the `person` objects.


### Reducing

A simple way of reducing is to create an array `Int(s)` and calculate the sum of all the values.

```
let numbers = [1, 5, 6, 8, 10]
```
**Old way**

```
var sum = 0
for num in numbers {
    sum += num
}
```
**Fun way** [Using `reduce`]

```
let sum = numbers.reduce(0, +)
```
The content of `sum` will be `30` for both cases.


The `reduce` method can compress a sequence into a single value, the random `0` is an inital value used to initialise the result. The `+` in our case is the closure that is applied to combine the partial results and the `n-th` element of the array.

```
partialResult = 0 + 1	// 1
partialResult = 1 + 5 	// 6
partialResult = 6 + 6 	// 12
partialResult = 12 + 8	// 20
partialResult = 20 + 3	// 30
```

After the last iteration partialResult can be considered the final result and its value will be `30`.

Here is a [link](https://useyourloaf.com/blog/swift-guide-to-map-filter-reduce/) to some more complex examples if you want to read a bit more.







