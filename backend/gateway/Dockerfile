# ---- Build Stage ----
FROM maven:3.9.6-eclipse-temurin-21 AS builder
WORKDIR /app

# Copy parent POM first (for dependency caching)
COPY pom.xml .

# Copy child module POM and source
COPY gateway/pom.xml gateway/pom.xml
COPY gateway/src gateway/src

# Install parent POM without building child modules
RUN mvn install -N -DskipTests --no-transfer-progress

# Build gateway
RUN mvn package -pl gateway -DskipTests --no-transfer-progress

# ---- Runtime Stage ----
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

COPY --from=builder /app/gateway/target/gateway-*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
