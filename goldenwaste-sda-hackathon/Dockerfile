FROM maven:3.8.6-openjdk-11
ENV GOLDEN_DB_URL=db
ENV GOLDEN_DB_PORT=3306
ENV GOLDEN_DB_NAME=goldenwaste
ENV GOLDEN_DB_USERNAME=root
ENV GOLDEN_DB_PASSWORD=edwin2023
WORKDIR /app
COPY . . 
RUN mvn package
EXPOSE 8080
ENTRYPOINT [ "java", "-jar", "target/goldenwaste.jar" ]