# Use GraalVM 25 for building the application
FROM ghcr.io/graalvm/jdk-community:25 AS build

# Set the working directory
WORKDIR /app

# Copy the Maven wrapper, project configuration, and source files
COPY ./.mvn /app/.mvn
COPY ./mvnw /app/mvnw
COPY ./pom.xml /app/pom.xml
COPY ./src /app/src

# Build the application
RUN chmod +x /app/mvnw && /app/mvnw clean package -Dmaven.test.skip=true

# Create a new image for running the application on GraalVM 25
FROM ghcr.io/graalvm/jdk-community:25

# Set the working directory
WORKDIR /app

# Copy the built JAR file from the previous stage
COPY --from=build /app/target/*.jar app.jar

# Expose the port on which the Spring Boot application will run
EXPOSE 8080

# Command to run the application
CMD ["java", "-jar", "app.jar"]
