# ---- Build stage ----
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

# Cache dependencies separately from source for faster rebuilds
COPY pom.xml .
RUN mvn dependency:go-offline -B

COPY src ./src
RUN mvn clean package -DskipTests -B

# ---- Runtime stage ----
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Run as non-root
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

COPY --from=build /app/target/*.jar app.jar

EXPOSE 9000

ENTRYPOINT ["java", "-jar", "/app/app.jar"]
