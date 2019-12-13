# Sample code to illustrate spring spring-cloud gateway framework

The idea is to utilize only open-source frameworks, tools and applications
to test the validity of spring-security as a proxy to different URL's


## Requirement
* Java 11
* Docker

## Installation

* First clone the project using git
* Validate you have java jdk11
```cmd
java --version
```
output
```txt
java 11.0.4 2019-07-16 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.4+10-LTS)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.4+10-LTS, mixed mode)
```

* Build docker images
  + Api-Gateway
    ```cmd
    cd api-gateway
    gradlew clean build jibDockerBuild
    cd ..
    ```

* Start docker-compose and all applications
```cmd
    docker-compose up -d
```

# Validate Applications is up


## Test the Application

The following are the 4 endpoints I tested:

```url
http://localhost:8280/test
```

response:

```txt
404 - Not Found
```

logs show:

```txt
2019-12-12 20:26:27.013 TRACE 44696 --- [ctor-http-nio-2] o.s.w.s.adapter.HttpWebHandlerAdapter    : [2c107ac2] HTTP GET "/test", headers={masked}
2019-12-12 20:26:27.013  INFO 44696 --- [ctor-http-nio-2] c.e.l.server.filter.LoggingWebFilter     : *** Request /test called ***
2019-12-12 20:26:27.178 TRACE 44696 --- [ctor-http-nio-2] o.s.w.s.adapter.HttpWebHandlerAdapter    : [2c107ac2] Completed 404 NOT_FOUND, headers={masked}
2019-12-12 20:26:27.178 TRACE 44696 --- [ctor-http-nio-2] org.springframework.web.HttpLogging      : [2c107ac2] Handling completed
```
```url
http://localhost:8280/foo/
```

response:

```txt
404 - Not Found
```

logs show:
```txt
2019-12-12 20:29:23.631 TRACE 44696 --- [ctor-http-nio-2] o.s.w.s.adapter.HttpWebHandlerAdapter    : [2c107ac2] HTTP GET "/foo/", headers={masked}
2019-12-12 20:29:23.632  INFO 44696 --- [ctor-http-nio-2] c.e.l.server.filter.LoggingWebFilter     : *** Request /foo/ called ***
2019-12-12 20:29:23.710 TRACE 44696 --- [ctor-http-nio-2] o.s.w.s.adapter.HttpWebHandlerAdapter    : [2c107ac2] Completed 404 NOT_FOUND, headers={masked}
2019-12-12 20:29:23.711 TRACE 44696 --- [ctor-http-nio-2] org.springframework.web.HttpLogging      : [2c107ac2] Handling completed
```
```url
http://localhost:8280/foo
```

```
Whitelabel Error Page
This application has no configured error view, so you are seeing this as a fallback.

Thu Dec 12 20:30:48 CST 2019
[2c107ac2] There was an unexpected error (type=Internal Server Error, status=500).
The path does not have a leading slash.
java.lang.IllegalArgumentException: The path does not have a leading slash.
	at org.springframework.util.Assert.isTrue(Assert.java:118)
	Suppressed: reactor.core.publisher.FluxOnAssembly$OnAssemblyException:
Error has been observed at the following site(s):
	|_ checkpoint ⇢ com.example.library.server.filter.LoggingWebFilter [DefaultWebFilterChain]
	|_ checkpoint ⇢ org.springframework.cloud.gateway.filter.WeightCalculatorWebFilter [DefaultWebFilterChain]
	|_ checkpoint ⇢ org.springframework.boot.actuate.metrics.web.reactive.server.MetricsWebFilter [DefaultWebFilterChain]
	|_ checkpoint ⇢ HTTP GET "/foo" [ExceptionHandlingWebHandler]
Stack trace:
```
```url
http://localhost:8280/actuator/
```

response:
```json
{"_links":{"self":{"href":"http://localhost:8280/actuator","templated":false},"gateway":{"href":"http://localhost:8280/actuator/gateway","templated":false}}}
```

going to
http://localhost:8280/actuator/gateway


response:
```txt
Whitelabel Error Page
This application has no configured error view, so you are seeing this as a fallback.

Thu Dec 12 20:25:51 CST 2019
[2c107ac2] There was an unexpected error (type=Not Found, status=404).
org.springframework.web.server.ResponseStatusException: 404 NOT_FOUND
	at org.springframework.web.reactive.resource.ResourceWebHandler.lambda$handle$0(ResourceWebHandler.java:325)
	Suppressed: reactor.core.publisher.FluxOnAssembly$OnAssemblyException:
Error has been observed at the following site(s):
	|_ checkpoint ⇢ com.example.library.server.filter.LoggingWebFilter [DefaultWebFilterChain]
	|_ checkpoint ⇢ org.springframework.cloud.gateway.filter.WeightCalculatorWebFilter [DefaultWebFilterChain]
	|_ checkpoint ⇢ org.springframework.boot.actuate.metrics.web.reactive.server.MetricsWebFilter [DefaultWebFilterChain]
	|_ checkpoint ⇢ HTTP GET "/actuator/gateway" [ExceptionHandlingWebHandler]
```

