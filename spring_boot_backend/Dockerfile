# syntax=docker/dockerfile:1

# ---- Build stage ----
FROM eclipse-temurin:17-jdk AS builder
WORKDIR /app

# Copy Gradle wrapper and build files
COPY build.gradle settings.gradle gradlew gradlew.bat ./
COPY gradle ./gradle

# Pre-fetch dependencies (speeds up subsequent builds)
RUN ./gradlew --no-daemon dependencies || true

# Copy source
COPY src ./src

# Build the application JAR
RUN ./gradlew --no-daemon clean bootJar

# ---- Runtime stage ----
FROM eclipse-temurin:17-jre
WORKDIR /app

# Copy the built jar from the builder stage
COPY --from=builder /app/build/libs/*.jar /app/app.jar

# Expose default Spring Boot port
EXPOSE 8080

# Start command as requested
CMD ["java", "-jar", "app.jar"]
