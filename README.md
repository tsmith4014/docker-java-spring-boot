# GoldenWaste Dockerization Guide

## Introduction

This guide is tailored for those new to Docker and the Java Spring Boot stack. It explains how to Dockerize the GoldenWaste project using both manual commands and automated Docker Compose configurations. This dual approach offers a comprehensive understanding of container deployment, beneficial for beginners.

## Application Configuration Overview

The `application.properties` file in the project is crucial for database connectivity.

### Key Snippets from application.properties

```properties
spring.datasource.url=jdbc:mysql://${GOLDEN_DB_URL}:${GOLDEN_DB_PORT}/${GOLDEN_DB_NAME}
spring.datasource.username=${GOLDEN_DB_USERNAME}
spring.datasource.password=${GOLDEN_DB_PASSWORD}
```

These properties are essential for configuring the application's connection to the database.

## Docker Configuration

### Dockerfile Breakdown

The Dockerfile is designed to build the Spring Boot application into a Docker image.

```Dockerfile
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
```

### Docker Compose File

The `docker-compose.yml` file orchestrates the application and its database service.

```yaml
version: "3"
services:
  app:
    container_name: golden-app
    build: .
    ports:
      - "8080:8080"
    environment:
      - GOLDEN_DB_URL=db
      - GOLDEN_DB_PASSWORD=DevOps
      - GOLDEN_DB_USERNAME=root
      - GOLDEN_DB_NAME=goldenwaste
      - GOLDEN_DB_PORT=3306
    depends_on:
      - db

  db:
    image: "mysql"
    container_name: goldenwastesqldb
    ports:
      - "3306:3306"
    environment:
      - MYSQL_DATABASE=goldenwaste
      - MYSQL_ROOT_PASSWORD=DevOps
```

## Manual vs. Automated Deployment

### Manual Docker Deployment

The `docker_run` file contains commands to manually create a network and run the containers:

```bash
docker network create goldenwaste-network
docker run -d --network goldenwaste-network --name goldenwastesqldb -e MYSQL_ROOT_PASSWORD=DevOps -e MYSQL_DATABASE=goldenwaste mysql
docker build -t golden .
docker run -itd --name golden-app -p 9876:8080 --network goldenwaste-network -e GOLDEN_DB_URL=goldenwastesqldb -e GOLDEN_DB_PASSWORD=DevOps -e GOLDEN_DB_USERNAME=root -e GOLDEN_DB_NAME=goldenwaste -e GOLDEN_DB_PORT=3306 golden
```

These commands:

1. Create a dedicated Docker network.
2. Start the MySQL database in a container.
3. Build the application Docker image.
4. Run the application container, connecting it to the MySQL container.

### Automated Deployment with Docker Compose

Alternatively, Docker Compose automates the setup process:

1. **Starting the Services**:

   - Run `docker-compose up` to start both the application and database services as defined in the `docker-compose.yml` file.
   - This command builds the application image and starts the containers based on the configuration.

2. **Stopping the Services**:
   - To stop the containers, use `docker-compose down`. This command stops and removes the containers, networks, and volumes created by `docker-compose up`.

### Benefits of Docker Compose

- **Simplicity**: Docker Compose simplifies the process of running multi-container Docker applications.
- **Configuration**: All configurations are in a YAML file, making it easy to adjust settings.
- **Reproducibility**: Ensures a consistent environment and setup for every instance of the application.

## Conclusion

This guide provides a comprehensive view of Dockerizing the GoldenWaste project, catering to those new to Docker and the Java Spring Boot stack. It outlines both manual and automated approaches using Docker Compose, offering flexibility and ease of use in deploying the application. By following these steps, you'll be able to set up and run the GoldenWaste project in a Dockerized environment efficiently.

---
