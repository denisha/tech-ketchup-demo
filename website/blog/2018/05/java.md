# Java

## Java 9 Features

Continuing from our previous post, we'll delve into more new features of JDK 9.
These are outlined below:


### What's New

#### 3. Http client: Finally, a new http client (jdk.incubator.http.HttpClient) is bundled in to replace the ageing HttpURLConnection. It is an experimental feature, so could be adopted or dropped in a future JDK release.
	It supports both HTTP/2 and WebSocket, and employs the builder pattern to make it far more easier to use. It is a good alternative to the likes of Apache HttpClient, particularly in terms of performance.
	
	Example implementation:
	
	HttpRequest request = HttpRequest.newBuilder()
		.uri(new URI("http://my-uri.com"))
		.GET()
		.build();
	
	HttpResponse<String> response = HttpClient.newHttpClient()
		.send(request, HttpResponse.BodyHandler.asString());

#### 4. Streaming an Optional:  It is now possible to stream a list of Optional objects as follows:
	List<String> list = optionalsList.stream()
		.flatMap(Optional::stream)
		.collect(Collectors.toList());
		

#### 5. Immutable Set/List: java.util.Set.of() and java.util.List.of() now allows for the creation of an immutable set/list of objects with a single line of code.
	
	Example implementation:
	
	Set<String> strKeySet = Set.of("val1", "val2", "val3");
	List<String> strKeySet = List.of("val1", "val2", "val3");


#### 6. Stream API improvements: Four new methods are added to the Stream API. viz. dropWhile, takeWhile, ofNullable, iterate
	
	Often, we know we want to keep taking or dropping elements while a certain condition is met, but we don’t necessarily know how many of these element we get.

	Example implementation:
	
	IntStream.range(0,10).takeWhile(x -> x < 5).forEach(System.out::println);   => Prints 0 1 2 3 4
	
	IntStream.range(0,10).dropWhile(x -> x < 5).forEach(System.out::println);   => Prints 5 6 7 8 9

	IntStream.iterate() is overloaded with the ability to provide a predicate that functions like the conditions of a traditional for-loop.
	
	Example implementation:
	
	IntStream.iterate(1, i -> i < 100, i -> i + 1).forEach(System.out::println);
