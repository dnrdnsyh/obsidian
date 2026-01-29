```
FROM 10.1.50.170/library/maven:3.9.8-21-icon AS build

WORKDIR /app
COPY pom.xml ./

# Cache dependency agar build berikutnya cepat
RUN mvn -B dependency:go-offline 
COPY src ./src

# RUN mvn -e --settings settings.xml clean package -DskipTests -Dquarkus.package.type=uber-jar -B
RUN mvn -e clean package \
    -DskipTests \
    -Dmaven.antrun.skip=true \
    -Dquarkus.package.type=uber-jar \
    -B

FROM 10.1.50.170/library/eclipse-temurin:21-jdk
WORKDIR /app/
RUN chown 1001 /app && chmod "g+rwX" /app && chown 1001:root /app \
    && mkdir /log && chmod "g+rwX" /log && chown 1001:root /log

COPY --from=build /app/target/*.jar /app/app.jar
COPY .env /app/

# Kita gunakan ENTRYPOINT agar argumen dari 'command' di compose otomatis ditambahkan di akhir
ENTRYPOINT ["java", "-jar", "app.jar"]
USER 1001
```