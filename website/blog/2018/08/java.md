# Java

## Spring Boot 2.0

Seeing as we're in the middle of upgrading the Old Mutual codebase to utilize the recently released version of Spring Boot, here follows a few of it's enhancements and benefits:

* **Java 8 baseline, and Java 9 support** All the jars ship with automatic module name entries in the manifests for module system compatibility.
* **3rd party library upgrades** Numerous security vulnerabilities as well as performance improvements have been addressed in updates
* **Reactive web programming support with Spring WebFlux/WebFlux.fn** Spring Boot provides auto-configuration for both annotation based Spring WebFlux applications, as well as WebFlux.fn which offers a more functional style API.
* **Auto-configuration and starter POMs for reactive Redis**
* **Reactive Spring** Reactive applications (which are now fully asynchronous and non-blocking) are intended for use in an event-loop execution model (instead of the more traditional one thread-per-request execution model). This is fully supported via auto-configuration and starter-POMs.
* **HTTP/2 is now provided for Tomcat**
* **Spring Boot’s Gradle plugin** This has been rewritten to enable various improvements.
* **Actuator Improvements** JSON payloads have been improved that now more accurately reflects the underlying data. Scheduled tasks (i.e. @EnableScheduling) can be be reviewed using the scheduledtasks actuator endpoint.
* **Data Support** The default database pooling technology in Spring Boot 2.0 has been switched from Tomcat Pool to HikariCP, that is found to offer superior performance.
