# Getting Started

### Startup Performance Test

The purpose of this test is to be testing how fast an app can be starting with springboot

```console
docker network create my-net
docker run --rm -p 3306:3306 --name mysql --net=my-net -e MYSQL_ROOT_PASSWORD=rootpass -e MYSQL_USER=db_user -e MYSQL_PASSWORD=db_pass -e MYSQL_DATABASE=sample-db -d mysql:5.6.51
./gradlew clean build
docker build -t sample-app:startUpPerformanceTest .
docker run --rm -d -p 8080:8080 -e MYSQL_HOST=mysql --name sample-app --net=my-net sample-app:startUpPerformanceTest
```

Without any tuning, the first results:

`Started StartUpPerformanceTestApplication in 9.215 seconds (JVM running for 10.644)`

Lets start with [Lazy Initialization](https://www.baeldung.com/spring-boot-lazy-initialization) inside the project.

Inside application.yaml

```console
spring:
  main:
    lazyInitialization: true
```

With lazy initialization:

`Started StartUpPerformanceTestApplication in 4.017 seconds (JVM running for 4.461)`

We can remove the lazyInitialization and go over autoconfiguration settings.

Starting with application.yaml:
```console
logging:
  level:
    org:
      springframework:
        boot:
          autoconfigure: DEBUG
```
And we now we should run `./gradlew clean build`

And then `java -jar build/libs/startUpPerformanceTest-startUpPerformance.jar`