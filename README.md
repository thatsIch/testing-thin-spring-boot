# Testing Thin Spring Boot

With Docker it is advised to layer your application from the least to the most frequent changing part. Using Spring Boot generally only allowed us to package the application in an Uber Jar (a Jar containing everything required to run the application thus including the servlet container and all Spring Framework dependencies). This lead to unnecessary fat Docker layers of multiple MBs even though only few KBs changed.
The best setup currently supported is the War packaging which excludes the servlet container saving about 3 MB.
The [project by dyser](https://github.com/dsyer/spring-boot-thin-launcher) allows the creation of a thin Spring Boot application by replacing all the dependencies by a custom wrapper which resolves the dependencies though Maven upon runtime.

This project test the plugin with a simple setup to showcase it is working.

## Getting Started

This project contains the Maven wrapper thus you are not required to have a Maven installed. So run

    mvnw clean package

which results in a `jar` in the `/target` folder. Run that `jar` with

    java -jar demo-0.0.1-SNAPSHOT.jar

which should start up a simple Spring Boot application. 

## Results

The business logic is around 3 KB denoted by `demo-0.0.1-SNAPSHOT.jar.original` and with the special wrapper around 9 KB (`demo-0.0.1-SNAPSHOT.jar`).
On each iteration cycle this would save about 13 MB - 9 KB space. Given more dependencies this would achieve even better results. 

## Consequences

Due to the new packaging style we need to change the handling with Docker. Now you cannot just copy the `jar` and execute it. You would need to layer your Maven Repository or mount it to share it with your coworkers. You can also take a dual route for using the thin wrapper for development and the uber jar for production. This should be decided by your business requirements.

