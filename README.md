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


## Create client app and test user in Keycloak
Once in Keycloak create the sgateway client and with client secret value: secret

Create also a test user

## Test the Application

The endpoint sgate/test1 is configured in the proxy to resolve to http://gateway:8280/test1 which in the Api-Gateway will resolve to
http://hello-service/hello

However, the Api-Gateway is a protected Client by the IDP which can be configured to be Okta or KeyCloak in the Api-Gateway application.yml file through the declartive yaml node spring.profiles.include with values:  okta, gateway-routes or keycloak, gateway-routes
As a result for the above request the user will get redirected to the IDP for the completion of Authentication.
Once the user is logged in, the user will get redirected to the original request which will eventually get full filled by the hello-service.
As the request gets routed to the hello-service the Identity is propagated down to the service by the Api Gateway via the Bearer-Token.

The reponse should be:
```

```