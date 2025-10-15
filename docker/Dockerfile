# Build stage: compile the application and package the Spring Boot jar
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /workspace

# Copy the Maven project files first to leverage Docker layer caching when
# dependencies have not changed.
COPY chat2db-server/pom.xml chat2db-server/pom.xml
COPY chat2db-server/chat2db-server-web-start/pom.xml chat2db-server/chat2db-server-web-start/pom.xml

# Copy the full server sources needed to build the Spring Boot web-start module.
COPY chat2db-server chat2db-server

RUN mvn -f chat2db-server/pom.xml -pl chat2db-server-web-start -am package -DskipTests

# Runtime stage: run the packaged jar with JDK 17
FROM openjdk:17
WORKDIR /app
COPY --from=builder /workspace/chat2db-server/chat2db-server-web-start/target/chat2db-server-web-start.jar chat2db-server-web-start.jar
# Let the current container expose 10824
EXPOSE 10824
# Run Jar
ENTRYPOINT ["java","-Dloader.path=lib","-Dspring.profiles.active=release","-jar","chat2db-server-web-start.jar"]
