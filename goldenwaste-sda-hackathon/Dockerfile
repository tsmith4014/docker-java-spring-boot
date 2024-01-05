FROM maven:3.8.6-openjdk-11
ENV GOLDEN_DB_URL=localhost
ENV GOLDEN_DB_PORT=3306
ENV GOLDEN_DB_NAME=goldenwaste
ENV GOLDEN_DB_USERNAME=admin
ENV GOLDEN_DB_PASSWORD=az98Yzaprics
WORKDIR /app
COPY . . 
RUN mvn package
EXPOSE 8080
ENTRYPOINT [ "java", "-jar", "target/goldenwaste.jar" ]