# Use current LTS JRE for production security and performance
FROM eclipse-temurin:21-jre
WORKDIR /app

# Copy verified local JAR to avoid registry/API redirect issues
COPY Geyser.jar /app/Geyser.jar
RUN chmod +x /app/Geyser.jar

# Map configuration to the persistent volume mount
CMD ["java", "-jar", "Geyser.jar", "--config", "/app/config/config.yml"]
