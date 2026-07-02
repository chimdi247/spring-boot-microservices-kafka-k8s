FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

COPY pom.xml .
COPY notification-service/pom.xml notification-service/pom.xml
WORKDIR /app/notification-service
RUN mvn dependency:go-offline -B

COPY notification-service/src ./src
RUN mvn package -DskipTests

FROM eclipse-temurin:21-jre-alpine AS stage-1
WORKDIR /app

RUN addgroup -S spring && adduser -S spring -G spring
USER spring

COPY --from=build /app/notification-service/target/*.jar app.jar

EXPOSE 8083
ENTRYPOINT ["java", "-jar", "app.jar"]
