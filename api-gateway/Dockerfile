# ---- Build stage ----
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

# Copy parent POM first - needed to resolve <parent> in the module POM
COPY pom.xml .
COPY api-gateway/pom.xml api-gateway/pom.xml
WORKDIR /app/api-gateway
RUN mvn dependency:go-offline -B  -DdockerPassword=unused

# Now bring in source and build
COPY api-gateway/src ./src
RUN mvn package -DskipTests  -DdockerPassword=unused

# ---- Runtime stage ----
FROM eclipse-temurin:21-jre-alpine AS stage-1
WORKDIR /app

RUN addgroup -S spring && adduser -S spring -G spring
USER spring

COPY --from=build /app/api-gateway/target/*.jar app.jar

EXPOSE 9000
ENTRYPOINT ["java", "-jar", "app.jar"]
