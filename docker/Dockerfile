# Build stage: compile the application and package the Spring Boot jar
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /workspace
COPY . .
RUN mvn -pl chat2db-server/chat2db-server-web-start -am package -DskipTests

# Runtime stage: run the packaged jar with JDK 17
FROM openjdk:17
WORKDIR /app
COPY --from=builder /workspace/chat2db-server/chat2db-server-web-start/target/chat2db-server-web-start.jar chat2db-server-web-start.jar
# Let the current container expose 10824
EXPOSE 10824
# Run Jar
ENTRYPOINT ["java","-Dloader.path=lib","-Dspring.profiles.active=release","-jar","chat2db-server-web-start.jar"]
