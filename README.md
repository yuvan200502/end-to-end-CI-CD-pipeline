# End-to-End CI/CD Pipeline Automation for Java Application

## Overview

This project implements an end-to-end Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Java Spring Boot application.

The pipeline automates source code management, application build, code quality analysis, security scanning, Docker image creation, and deployment to an AWS EC2 environment.

The project demonstrates practical implementation of DevOps tools and practices using GitHub, Jenkins, Maven, SonarQube, JaCoCo, Trivy, Docker, Docker Hub, and AWS EC2.

---

## Project Objectives

* Automate the application build and deployment process.
* Implement a multi-stage CI/CD pipeline using Jenkins.
* Integrate GitHub with Jenkins for continuous integration.
* Automate Java application builds using Maven.
* Perform static code quality analysis using SonarQube.
* Generate code coverage reports using JaCoCo.
* Perform container vulnerability scanning using Trivy.
* Build and publish Docker images.
* Deploy the containerized application to AWS EC2.
* Reduce manual intervention during application deployment.

---

## CI/CD Pipeline

```text
Developer
    |
    v
GitHub Repository
    |
    v
Jenkins
    |
    +----------------------+
    |                      |
    v                      v
Maven Build           SonarQube Analysis
    |                      |
    v                      v
Unit Tests             Quality Gate
    |                      |
    +----------+-----------+
               |
               v
        Docker Build
               |
               v
        Trivy Security Scan
               |
               v
        Docker Hub
               |
               v
           AWS EC2
               |
               v
      Spring Boot Application
```

---

## Pipeline Stages

### 1. Source Code Management

GitHub is used to store and manage the application source code.

Jenkins retrieves the latest source code from the GitHub repository whenever the pipeline is triggered.

### 2. Application Build

Maven is used to compile the Java application, execute tests, and package the application as a JAR file.

```bash
mvn clean package
```

The generated JAR file is stored in the `target` directory.

### 3. Code Quality Analysis

SonarQube is integrated into the pipeline to perform static code analysis.

The analysis identifies:

* Bugs
* Vulnerabilities
* Code smells
* Code duplication
* Maintainability issues
* Code coverage

A SonarQube Quality Gate can be used to determine whether the pipeline should continue.

### 4. Code Coverage

JaCoCo is integrated with Maven to generate code coverage reports.

The report is generated under:

```text
target/site/jacoco/
```

### 5. Security Scanning

Trivy is used to scan Docker images for known security vulnerabilities.

Example:

```bash
trivy image <docker-image>
```

### 6. Docker Image Creation

The Spring Boot application is containerized using Docker.

Example:

```bash
docker build -t database-service:latest .
```

### 7. Docker Image Registry

The Docker image is pushed to Docker Hub for storage and distribution.

Example:

```bash
docker login
docker push <dockerhub-username>/<image-name>:latest
```

### 8. AWS EC2 Deployment

The Docker image is deployed to an AWS EC2 instance.

Example:

```bash
docker pull <dockerhub-username>/<image-name>:latest

docker run -d \
  -p 8081:8080 \
  --name database-service \
  <dockerhub-username>/<image-name>:latest
```

The Spring Boot application runs on port `8080` inside the container and is exposed through port `8081` on the EC2 instance.

Jenkins runs separately on port `8080`, so port `8081` is used for the application to avoid a port conflict.

---

## Technologies Used

| Technology  | Purpose                         |
| ----------- | ------------------------------- |
| Java        | Application development         |
| Spring Boot | Application framework           |
| Git         | Version control                 |
| GitHub      | Source code repository          |
| Jenkins     | CI/CD automation                |
| Maven       | Build and dependency management |
| SonarQube   | Code quality analysis           |
| JaCoCo      | Code coverage                   |
| Trivy       | Security vulnerability scanning |
| Docker      | Application containerization    |
| Docker Hub  | Container image registry        |
| AWS EC2     | Application deployment          |
| Linux       | Server environment              |

---

## Project Structure

```text
end-to-end-CI-CD-pipeline/
|
├── src/
│   ├── main/
│   │   ├── java/
│   │   └── resources/
│   |
│   └── test/
|
├── target/
|
├── Dockerfile
├── Jenkinsfile
├── pom.xml
└── README.md
```

---

## Application Configuration

The project is configured with Java 11 as the application compilation target.

```xml
<java.version>11</java.version>
```

The Maven Compiler Plugin is configured to compile the application with Java 11 compatibility.

The Jenkins server uses JDK 21 and Maven 3.9.x.

This allows the application to maintain Java 11 compatibility while using a newer JDK on the CI/CD server.

---

## Prerequisites

The following tools are required to reproduce this project:

* Git
* GitHub account
* Java JDK
* Maven
* Jenkins
* SonarQube
* Trivy
* Docker
* Docker Hub account
* AWS account
* AWS EC2 instance
* Linux environment

---

## Build and Run Locally

Clone the repository:

```bash
git clone <YOUR-GITHUB-REPOSITORY-URL>
```

Navigate to the project:

```bash
cd end-to-end-CI-CD-pipeline
```

Build the application:

```bash
mvn clean package
```

Run the Spring Boot application:

```bash
java -jar target/*.jar --server.port=8081
```

The application can then be accessed at:

```text
http://localhost:8081
```

---

## Docker Deployment

Build the Docker image:

```bash
docker build -t database-service:latest .
```

Run the container:

```bash
docker run -d \
  -p 8081:8080 \
  --name database-service \
  database-service:latest
```

Verify the running container:

```bash
docker ps
```

Access the application:

```text
http://<EC2-PUBLIC-IP>:8081
```

---

## Jenkins Pipeline

The Jenkins pipeline automates the following workflow:

```text
Checkout
   |
   v
Maven Build
   |
   v
Unit Tests
   |
   v
JaCoCo Coverage
   |
   v
SonarQube Analysis
   |
   v
Quality Gate
   |
   v
Docker Build
   |
   v
Trivy Security Scan
   |
   v
Docker Push
   |
   v
AWS EC2 Deployment
```

---

## AWS EC2 Configuration

The application is deployed to an AWS EC2 Linux instance.

Typical ports used by the environment:

| Port | Service                 |
| ---: | ----------------------- |
|   22 | SSH                     |
| 8080 | Jenkins                 |
| 8081 | Spring Boot Application |
| 9000 | SonarQube               |

Security Group rules should be configured according to the deployment requirements. In production environments, unnecessary public access should be avoided.

---

## Security Considerations

Sensitive credentials should not be stored directly in the GitHub repository or Jenkinsfile.

The following should be managed using Jenkins Credentials, AWS IAM roles, or environment variables:

```text
AWS credentials
Docker Hub credentials
SonarQube credentials
SSH credentials
Repository tokens
```

Do not commit passwords, access keys, private keys, or tokens to the repository.

---

## Key DevOps Concepts Demonstrated

This project demonstrates practical knowledge of:

* Continuous Integration
* Continuous Deployment
* Jenkins Pipeline
* Git and GitHub
* Maven Build Automation
* Static Code Analysis
* Code Coverage
* Container Security
* Docker Containerization
* Docker Image Management
* Linux Administration
* AWS EC2 Deployment
* Automated Deployment

---

## Project Outcome

The completed pipeline provides an automated workflow from source code commit to application deployment.

```text
Code Commit
    |
    v
GitHub
    |
    v
Jenkins
    |
    v
Build and Test
    |
    v
SonarQube
    |
    v
Security Scan
    |
    v
Docker Image
    |
    v
Docker Hub
    |
    v
AWS EC2
    |
    v
Running Application
```

This eliminates repetitive manual build and deployment steps and provides a consistent software delivery process.

---

## Author

**Yuvan Venkatesan**

Computer Science Engineering
AWS and DevOps Engineer

---

## License

This project was developed for educational and learning

