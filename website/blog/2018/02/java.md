# Java 9 Features


With the advent of the latest release of the JDK, comes a host of new features along with improvements to various existing ones.
These will be briefly outlined over time in future newsletters, of which a couple are listed below:

### What's New

#### 1. JShell 
This is a new programming tool (packaged with the JDK) engineered for quick testing via the command line. Its also referred to as a REPL (Read Evaluate Print Loop) tool.
You'd write import statements followed by functional lines of code that may also include any Java construct such as classes, interfaces and enums.
On-the-fly calculations could be assigned to internal variables (given numeric names starting with $1) that could then be used in later code.
Some code completion (using the tab key) even exists, that behaves similar to an IDE to view available options such as existing variables or available classes.
		   
References with examples for further reading:
* [JShell ref 1](https://www.journaldev.com/9879/java-repl-jshell)
* [JShell ref 2](https://www.journaldev.com/12938/jshell-java-shell)


### What's Changed

#### 1. Try-with resources
Before, the class that implemented the AutoCloseable interface would have to be instantiated or re-assigned inside of the opening try block.
Now, the final or effectively final object may be instantiated outside of the try block and then just accessed inside of the try block.
		   
References with examples for further reading:
* [Try-with-resources ref 1](https://dzone.com/articles/try-with-resources-enhancement-in-java-9)
* [Try-with-resources ref 2](https://www.javatpoint.com/java-9-try-with-resources)
		   
#### 2. Interface private method
The introduction of the default method in interfaces has had a limitation in terms of its flexibility, as the default method could become bloated.
Private methods are now allowed in interfaces, and of course serves to benefit code-reuse as well as more abstracted code.

References with examples for further reading:
* [Interface private methods ref 1](https://howtodoinjava.com/java9/java9-private-interface-methods/)
* [Interface private methods ref 2](https://howtoprogram.xyz/2017/09/26/java-9-private-interface-methods/)