# Etapa 1: Construcción con Maven
FROM maven:3.8-openjdk-17 AS builder
WORKDIR /app
# Copiar el POM y descargar dependencias (cache)
COPY pom.xml .
RUN mvn dependency:go-offline -B
# Copiar el resto del código
COPY src ./src
# Empaquetar (saltando tests)
RUN mvn clean package -DskipTests

# Etapa 2: Imagen final con JRE
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
# Copiar el JAR generado
COPY --from=builder /app/target/*.jar app.jar
# Crear usuario no root
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]