# Stage 1: Build application with Maven
FROM maven:3.8.4-openjdk-17 AS builder

# Set working directory and copy file
WORKDIR /app
COPY pom.xml .
COPY src ./src

# Build the application
RUN mvn package -DskipTests

# Stage 2: Use JDK image only for runtime
FROM openjdk:17-jdk-slim

# Copy built JAR file from builder stage
COPY --from=builder /app/target/*.jar /app/app.jar

# Run the application
CMD ["java", "-jar", "/app/app.jar"]
