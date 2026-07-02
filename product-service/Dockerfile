FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

COPY pom.xml .
COPY product-service/pom.xml product-service/pom.xml
WORKDIR /app/product-service
RUN mvn dependency:go-offline -B  -DdockerPassword=unused

COPY product-service/src ./src
RUN mvn package -DskipTests  -DdockerPassword=unused

FROM eclipse-temurin:21-jre-alpine AS stage-1
WORKDIR /app

RUN addgroup -S spring && adduser -S spring -G spring
USER spring

COPY --from=build /app/product-service/target/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
