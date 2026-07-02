FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

COPY pom.xml .
COPY order-service/pom.xml order-service/pom.xml
WORKDIR /app/order-service
RUN mvn dependency:go-offline -B

COPY order-service/src ./src
RUN mvn package -DskipTests

FROM eclipse-temurin:21-jre-alpine AS stage-1
WORKDIR /app

RUN addgroup -S spring && adduser -S spring -G spring
USER spring

COPY --from=build /app/order-service/target/*.jar app.jar

EXPOSE 8081
ENTRYPOINT ["java", "-jar", "app.jar"]
