# ---- Build Stage ----
FROM maven:3.9.6-eclipse-temurin-21 AS builder
WORKDIR /app

# Copy parent POM first (for dependency caching)
COPY pom.xml .

# Copy child module POM and source
COPY payment-service/pom.xml payment-service/pom.xml
COPY payment-service/src payment-service/src

# Install parent POM without building child modules
RUN mvn install -N -DskipTests --no-transfer-progress

# Build payment-service
RUN mvn package -pl payment-service -DskipTests --no-transfer-progress

# ---- Runtime Stage ----
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

COPY --from=builder /app/payment-service/target/payment-service-*.jar app.jar

EXPOSE 8083
ENTRYPOINT ["java", "-jar", "app.jar"]
