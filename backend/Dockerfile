# Stage 1: Build
FROM openjdk:17-jdk-slim AS build
WORKDIR /app
COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn
RUN chmod +x mvnw
RUN ./mvnw dependency:go-offline
COPY src ./src
RUN ./mvnw clean package -DskipTests

# Stage 2: Test
FROM build AS test
RUN ./mvnw test

# Stage 3: Runtime
FROM openjdk:17-jre-slim AS runtime
WORKDIR /app
COPY --from=build /app/target/chattingo-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
