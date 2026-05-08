# ===== Build Stage =====
FROM maven:3.8.5-openjdk-8 AS build

WORKDIR /app

# Copy pom.xml files first for dependency caching
COPY pom.xml .
COPY vhrserver/pom.xml vhrserver/
COPY vhrserver/vhr-mapper/pom.xml vhrserver/vhr-mapper/
COPY vhrserver/vhr-model/pom.xml vhrserver/vhr-model/
COPY vhrserver/vhr-service/pom.xml vhrserver/vhr-service/
COPY vhrserver/vhr-web/pom.xml vhrserver/vhr-web/

# Download dependencies (cached layer)
RUN mvn dependency:go-offline -B -pl vhrserver/vhr-web -am || true

# Copy all source code
COPY . .

# Build only vhrserver (skip mailserver)
RUN mvn clean package -pl vhrserver/vhr-web -am -DskipTests -B

# ===== Runtime Stage =====
FROM eclipse-temurin:8-jre-jammy

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

WORKDIR /app

# Copy the built JAR from vhr-web module
COPY --from=build /app/vhrserver/vhr-web/target/vhr-web-*.jar app.jar

EXPOSE 8081

ENV SPRING_PROFILES_ACTIVE=docker

ENTRYPOINT ["java", "-jar", "app.jar"]
