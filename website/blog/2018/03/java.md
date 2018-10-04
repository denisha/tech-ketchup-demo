# Java

## Java 9 Features

Continuing from our previous post, we'll delve into one of the core new features of JDK 9, as well as an improvement for readability and type safety.
These are outlined below:


### What's New

#### 2. Modular system
This is the largest update that reforms the architecture of the Java platform. It's been designed to effect a lower memory footprint with the intention of being executable on a larger
variety of devices where available memory is minimal.

Implementation comprises of 2 parts: viz. the module declaration and the package hierarchy with its code. Example:

Declaration - src/com.example/module-info.java

			module com.example {
				exports com.example;
			}
			
Package hierarchy = src/com.example/com/example/DateUtil.java

			package com.example;
			public class DateUtil {
				public static LocalDate getCurrentDate() {
					return LocalDate.now();
				}
			}
		
Import - src/com.helloworld/module-info.java

			module com.helloworld {
				requires com.example;
			}
		
Usage - src/com.helloworld/com/helloworld/Main.java

			package com.helloworld;
			import com.example;
			
			public class Main {
				public static void main(String[] args) {
					System.out.println("The date today is " + DateUtil.getCurrentDate());
				}
			}
			
In the above example, a module named com.example is made visible to a module named com.helloworld so that it's code may be accessed directly.
		   
References for further reading:
* [Understanding modules](https://www.oracle.com/corporate/features/understanding-java-9-modules.html)
* [Quick start](http://openjdk.java.net/projects/jigsaw/quick-start)


### What's Changed

#### 3. Diamond operator support addition
Previously, anonymous classes didn't fully cater for type safety when using generics, and left a gap in type inference.
This has now been covered, as demonstrated in the following example:
	
	    public interface Hello<T> {
		    public String sayHello(T value);
	    }
	
	    public static void main(String[] args) {
		    Hello<String> hello = new Hello<> {
			    @Override
			    public String sayHello(String value) {
				    return "Hello " + value.toString();
			    }
		    }
	    }

References for further reading:
* [Diamond operator](http://slackspace.de/articles/java-9-diamond-operator-and-anonymous-classes/)
* [Diamond operator with anonymour inner classes](http://bytepadding.com/java/java-9-diamond-operator-with-anonymous-inner-classes/)