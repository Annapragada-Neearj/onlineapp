# ============================
#   1. Build Stage
# ============================
FROM maven:3.9.6-eclipse-temurin-21 AS builder

# Set working directory inside container
WORKDIR /app

# Copy only pom.xml first for caching
COPY onlinevegetablesale/onlinevegetablesale/pom.xml .

# Download dependencies
RUN mvn dependency:go-offline -B

# Now copy full source
COPY onlinevegetablesale/onlinevegetablesale/src ./src

# Build the application
RUN mvn clean package -DskipTests


# ============================
#   2. Runtime Stage
# ============================
FROM eclipse-temurin:21-jdk-jammy

WORKDIR /app

# Copy JAR from builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose application port
EXPOSE 8080

# Command to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
