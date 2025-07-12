# Étape 1 : Build de l'application
FROM hseeberger/scala-sbt:17.0.2_1.6.2_3.1.1 as builder

WORKDIR /app

# Copier les fichiers de build en premier pour profiter du cache Docker
COPY project ./project
COPY build.sbt .
COPY src ./src
COPY target ./target
COPY src/main/resources ./src/main/resources

# Compiler le projet et packager le JAR
RUN sbt clean assembly

# Étape 2 : Image finale minimale
FROM eclipse-temurin:17-jre

WORKDIR /app

# Copier le JAR depuis l'étape de build
COPY --from=builder /app/target/scala-2.13/data-processing-api-assembly-0.1.0-SNAPSHOT.jar app.jar
COPY --from=builder /app/src/main/resources ./resources

# Exposer le port (à adapter selon application.conf)
EXPOSE 8080

# Commande de lancement
CMD ["java", "-cp", "app.jar:resources", "com.sacala.dataprocessing.Main"] 