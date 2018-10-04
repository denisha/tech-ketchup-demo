# Java

## Java 10 Features

### Type Inference

With JEP 286, local variable type inference (JDK Enhancement Proposal), a new keyword, var, has been introduced, which shortens the declaration of a local variable. It indicates to the compiler to infer the type of the local variable from its initializer.

Prior to Java 10, we used to declare variables like this:

```
URL simpleProgrammer = new URL(http://www.simpleprogrammer.com);
URLConnection connection = simpleProgrammer.openConnection();
Reader reader = new BufferedReader(New InputStreamReader(connection.getInputStream()));
```

With Java 10, we can avoid explicit type declaration and write code like this:

```
var simpleProgrammer = new URL(http://www.simpleprogrammer.com);
var connection = simpleProgrammer.openConnection();
var reader = new BufferedReader(New InputStreamReader(connection.getInputStream()));
```

The keyword var has made Java less verbose by removing the redundancy from the variable declaration. It would be possible to implicitly determine the type of variable from the context in which it is used.

### Parallel full GC in G1

G1 GC was introduced in Java 8 and it became the default garbage collector in Java 9. By design, it avoids full garbage collections, but they still happen.

G1 uses only a single threaded mark-sweep-compact algorithm to perform a full collection, which can result in performance issues.

Java 10 has fixed this issue by performing full GC by using multiple threads. The same number of threads are used for full collection as the number of threads being used for young and mixed collections. There would be a marked improvement in full GC performance of the G1 collector now.

### Time-based release versioning

The format of the Java version number has been changed to improve the support for a time-based release model. The most notable aspect of the new release model is that the content of a release is subject to change.

At the beginning, only the release date is announced. However, if the development of this new feature takes longer than anticipated, it is removed from the release cadence and will not be included. Therefore, there is a need for a version number that depicts the passage of time, instead of the nature of the included changes.