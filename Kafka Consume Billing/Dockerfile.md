```
FROM maven:3.9.8-eclipse-temurin-22-jammy AS builder
WORKDIR /app
COPY . .
RUN mvn clean package \
    -Dquarkus.package.jar.enabled=true \
    -Dquarkus.package.type=uber-jar \
    -Dmaven.test.skip=true
# Stage 2: Run the application using JRE 21
FROM eclipse-temurin:21-jre
WORKDIR /app
RUN mkdir -p /app /log \
    && chmod g+rwX /app /log \
    && chown -R 1001:root /app /log
# INI yang benar: ambil dari stage builder
COPY --chown=1001:root --from=builder /app/target/*.jar /app/app.jar
COPY --chown=1001:root .env /app/
COPY --chown=1001:root start.sh /app/start.sh
RUN chmod +x /app/start.sh
EXPOSE 8008
CMD ["/app/start.sh"]
```