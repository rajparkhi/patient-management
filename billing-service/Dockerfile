FROM maven:3.9.9-eclipse-temurin-21 AS builder

WORKDIR /app

COPY pom.xml .
RUN mvn dependency:go-offline -B



# Make sure proto files are included
COPY src ./src

# Run full build to trigger gRPC codegen
RUN mvn clean install -DskipTests && \
    echo "✅ Checking generated gRPC sources..." && \
    find target/generated-sources/protobuf -type f -name "*.java" && \
    echo "✅ Done listing generated sources"

FROM openjdk:21-jdk AS runner

WORKDIR /app

COPY --from=builder /app/target/billing-service-0.0.1-SNAPSHOT.jar ./app.jar

EXPOSE 4001
EXPOSE 9001

ENTRYPOINT ["java", "-jar", "app.jar"]

