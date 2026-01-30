```
# Stage 1: Build  
FROM maven:3.9.6-eclipse-temurin-21 AS builder  
  
WORKDIR /build  
COPY . .  
  
# Build hanya module app  
RUN mvn clean package \  
    -pl app \  
    -am \  
    -Pdevelopment \  
    -DskipTests \  
    -Dquarkus.package.jar.type=uber-jar  
  
# Stage 2: Runtime  
FROM eclipse-temurin:21-jdk-alpine  
  
WORKDIR /app  
  
# Copy JAR dari module app  
COPY --from=builder /build/app/target/*.jar /app/app.jar  
  
EXPOSE 8080  
  
CMD ["java", \  
     "-Dmail.debug=true", \  
     "-Djava.net.preferIPv4Stack=true", \  
     "-Dquarkus.profile=development", \  
     "-jar", "app.jar"]
```