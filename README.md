## VProfile Web Application

### Overview
A Java Spring MVC web application for user profile management with:
- registration and login via Spring Security
- MySQL persistence using Spring Data JPA
- JSP-based UI pages
- file upload support
- RabbitMQ message publishing
- Elasticsearch indexing and lookup
- Memcached caching

This repository also includes:
- `Dockerfile` for containerized deployment
- `Jenkinsfile` for CI/CD pipeline
- `ansible/` automation playbooks
- `kubernetes/` GitOps deployment manifests

---

## Features
- User registration and authentication
- Role-based access control for protected pages
- View all users and individual user profiles
- Update user profile details
- Upload profile files from the web UI
- Cache user lookup results with Memcached
- Publish messages to RabbitMQ
- Index user data in Elasticsearch and query by ID

---

## Technology Stack
- Java 11
- Spring MVC, Spring Security, Spring Data JPA
- Hibernate
- MySQL
- JSP / Servlets
- RabbitMQ
- Elasticsearch
- Memcached
- Maven
- Tomcat 9
- Docker
- Jenkins
- Ansible
- Kubernetes

---

## Prerequisites
- JDK 11
- Maven 3
- MySQL 8
- Docker (optional)
- Jenkins / SonarQube / Nexus for CI/CD (optional)

---

## Local Setup

1. Clone the repository.

2. Configure MySQL:
   - Create the database:
     - `CREATE DATABASE accounts;`
   - Import the sample data:
     - `mysql -u <username> -p accounts < src/main/resources/db_backup.sql`

3. Update the database connection in `src/main/resources/application.properties`.
   Default values currently configured in the repo are:
   - `jdbc.url=jdbc:mysql://vprodb:3306/accounts?...`
   - `jdbc.username=root`
   - `jdbc.password=vprodbpass`

4. Build and run the app:
   - `mvn clean install`
   - `mvn jetty:run`

Or deploy the generated WAR to Tomcat.

---

## Docker Build
Build and run the app using Docker:

- Build:
  - `docker build -t vprofile .`
- Run:
  - `docker run --name vprofile -p 8080:8080 vprofile`

The provided `Dockerfile` builds the app with Maven on OpenJDK 11 and deploys the resulting WAR to Tomcat 9.

---

## Configuration

The main configuration file is `src/main/resources/application.properties`.
It includes settings for MySQL, Memcached, RabbitMQ, and Elasticsearch.

Default connection hostnames in the repository are environment-specific and should be replaced for local or cloud use:
- `vprodb`
- `vprocache01`
- `vprocache02`
- `vpromq01`
- `vprosearch01`

---

## CI/CD

The `Jenkinsfile` defines a build pipeline with these stages:
- Maven build and artifact archive
- Unit tests
- Integration tests
- Checkstyle analysis
- SonarQube analysis
- Artifact publishing to Nexus

---

## Repository Structure
- `pom.xml` — Maven build configuration
- `Dockerfile` — Docker image build instructions
- `Jenkinsfile` — CI/CD pipeline definition
- `src/main/java` — Java source code
- `src/main/resources` — application properties and SQL dump
- `src/main/webapp/WEB-INF` — Spring configuration and JSP views
- `ansible/` — deployment automation playbooks
- `kubernetes/` — Kubernetes manifests

---

## Notes
- `Spring Security` uses BCrypt password encoding.
- User profile data is stored in MySQL and cached with Memcached.
- Elasticsearch integration uses the transport client and the `users` index.
- RabbitMQ test messages are published by the `/user/rabbit` endpoint.
- File uploads are saved under Tomcat `${catalina.home}/tmpFiles`.
