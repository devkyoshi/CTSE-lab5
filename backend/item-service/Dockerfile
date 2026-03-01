# ---- Build Stage ----
FROM maven:3.9.6-eclipse-temurin-21 AS builder
WORKDIR /app

# Copy parent POM first (for dependency caching)
COPY pom.xml .

# Copy child module POM and source
COPY item-service/pom.xml item-service/pom.xml
COPY item-service/src item-service/src

# Install parent POM without building child modules
RUN mvn install -N -DskipTests --no-transfer-progress

# Build item-service
RUN mvn package -pl item-service -DskipTests --no-transfer-progress

# ---- Runtime Stage ----
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

COPY --from=builder /app/item-service/target/item-service-*.jar app.jar

EXPOSE 8081
ENTRYPOINT ["java", "-jar", "app.jar"]
