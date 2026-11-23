# Automated CI/CD Delivery Platform

An end-to-end CI/CD and DevOps project demonstrating automated build, testing, Docker image publishing, application deployment, and monitoring using Java 17, Spring Boot, Docker, GitHub Actions, Prometheus, and Grafana.

## Project Overview

The Automated CI/CD Delivery Platform automates the software delivery lifecycle for a Spring Boot Employee Management application.

Whenever code is pushed to the GitHub repository, GitHub Actions automatically builds and tests the application, creates a Docker image, publishes it to GitHub Container Registry (GHCR), and deploys the application through a Windows self-hosted runner.

The platform also integrates Prometheus and Grafana for application monitoring and visualization.

## Key Features

- RESTful Employee Management APIs.
- Complete CRUD operations.
- MySQL database integration using Spring Data JPA.
- Automated Maven build and test execution.
- Docker-based application containerization.
- Automated CI/CD using GitHub Actions.
- Docker image publishing to GitHub Container Registry.
- Automated deployment using a Windows self-hosted runner.
- Docker Compose service orchestration.
- Prometheus application metrics collection.
- Grafana monitoring dashboard.
- Spring Boot Actuator health and monitoring endpoints.
- Persistent MySQL storage using Docker volumes.

## Technology Stack

| Category | Technology |
|---|---|
| Programming Language | Java 17 |
| Backend Framework | Spring Boot |
| ORM | Spring Data JPA / Hibernate |
| Database | MySQL 8.0 |
| Build Tool | Maven |
| API Testing | Postman |
| Containerization | Docker |
| Orchestration | Docker Compose |
| CI/CD | GitHub Actions |
| Container Registry | GitHub Container Registry |
| Deployment | Windows Self-Hosted Runner |
| Monitoring | Prometheus |
| Visualization | Grafana |
| Health Monitoring | Spring Boot Actuator |
| Version Control | Git and GitHub |
| Development Environment | Visual Studio Code |

## System Architecture

The platform follows this automated delivery workflow:

1. Developer makes application code changes.
2. Code is pushed to the GitHub repository.
3. GitHub Actions triggers the CI/CD workflow.
4. Maven builds the application and executes tests.
5. Docker builds the application image.
6. The image is published to GitHub Container Registry.
7. The Windows self-hosted runner receives the deployment job.
8. Docker Compose pulls the latest application image and deploys the application.
9. Spring Boot communicates with MySQL for persistent data storage.
10. Prometheus collects application metrics.
11. Grafana displays monitoring metrics through dashboards.

## CI/CD Pipeline

The GitHub Actions workflow consists of three major jobs.

### 1. Build and Test

- Checks out the source code.
- Starts a MySQL service for integration testing.
- Builds the Spring Boot application using Maven.
- Executes automated tests.
- Generates the application JAR artifact.

### 2. Build and Publish Docker Image

- Builds the Docker image.
- Tags the image for GitHub Container Registry.
- Publishes the image to GHCR.

### 3. Deploy to Windows Self-Hosted Runner

- Executes on the configured Windows self-hosted runner.
- Pulls the latest Docker image.
- Deploys the application using Docker Compose.
- Verifies the deployed application health.

## Docker Architecture

The application uses Docker Compose to manage four services:

| Service | Container | Port |
|---|---|---|
| Spring Boot Application | employee-app | 8080 |
| MySQL Database | employee-mysql | 3307 |
| Prometheus | prometheus | 9090 |
| Grafana | grafana | 3000 |

MySQL uses a persistent Docker volume to retain database records across container restarts.

## Monitoring and Observability

### Prometheus

Prometheus collects application metrics from the Spring Boot Actuator Prometheus endpoint.

Metrics endpoint:

`http://localhost:8080/actuator/prometheus`

Prometheus dashboard:

`http://localhost:9090`

The Prometheus configuration is maintained in:

`prometheus/prometheus.yml`

### Grafana

Grafana provides a dashboard for visualizing application performance metrics.

Dashboard:

`http://localhost:3000`

The monitoring dashboard includes:

- JVM Memory Usage
- System CPU Usage
- Application Process CPU Usage
- Employee API Requests Per Second

Prometheus is configured as the Grafana data source.

## Employee Management REST APIs

Base URL:

`http://localhost:8080/api/employees`

| HTTP Method | Endpoint | Description |
|---|---|---|
| GET | `/api/employees` | Retrieve all employees |
| GET | `/api/employees/{id}` | Retrieve employee by ID |
| POST | `/api/employees` | Create a new employee |
| PUT | `/api/employees/{id}` | Update employee details |
| DELETE | `/api/employees/{id}` | Delete an employee |

### Sample Employee JSON

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "department": "Engineering",
  "salary": 75000.00
}
```

## Database Configuration

The application uses MySQL as its relational database and Spring Data JPA for database operations.

Database name:

`employee_management`

Database credentials are supplied through environment variables.

Example application configuration:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/employee_management
spring.datasource.username=root
spring.datasource.password=${DB_PASSWORD}
spring.jpa.hibernate.ddl-auto=update
```

Never commit actual passwords, API tokens, or other credentials to the repository.

## Running the Application Locally

### Prerequisites

- Java 17 or later
- Docker Desktop
- Docker Compose
- Git
- PowerShell or terminal
- GitHub account

### Step 1: Clone the repository

```bash
git clone https://github.com/Mayor441/automated-cicd-delivery-platform.git
cd automated-cicd-delivery-platform
```

### Step 2: Configure environment variables

Create a `.env` file in the project root.

Configure the required database variables:

```env
DB_PASSWORD=your_application_password
MYSQL_ROOT_PASSWORD=your_root_password
```

Use your own secure passwords. Do not commit the `.env` file.

### Step 3: Start the application

From the project root, run:

```powershell
docker compose up -d
```

### Step 4: Verify running containers

```powershell
docker compose ps
```

Check that the application and database containers are healthy and the monitoring services are running.

### Step 5: Test the application

Open:

`http://localhost:8080/api/employees`

## Actuator Health Endpoints

The application exposes the following monitoring endpoints:

| Endpoint | Purpose |
|---|---|
| `/actuator/health` | Application health |
| `/actuator/info` | Application information |
| `/actuator/metrics` | Application metrics |
| `/actuator/prometheus` | Prometheus metrics |

Health endpoint:

`http://localhost:8080/actuator/health`

Detailed health component information is disabled in the current configuration.

## Project Structure

```text
automated-cicd-delivery-platform/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── springboot-app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── com/cicd/employee/
│   │   │   │       ├── controller/
│   │   │   │       ├── entity/
│   │   │   │       ├── repository/
│   │   │   │       ├── service/
│   │   │   │       └── DemoApplication.java
│   │   │   └── resources/
│   │   │       └── application.properties
│   │   └── test/
│   ├── Dockerfile
│   ├── pom.xml
│   └── mvnw
│
├── prometheus/
│   └── prometheus.yml
│
├── docker-compose.yml
├── .gitignore
└── README.md
```

## Verification and Testing

The following functionality has been verified:

- Spring Boot application builds successfully.
- Employee CRUD APIs work successfully.
- MySQL stores and retrieves employee records.
- Employee data persists across container restarts.
- Docker containers run successfully.
- GitHub Actions build and test job succeeds.
- Docker image publishing job succeeds.
- Self-hosted runner deployment job succeeds.
- Prometheus target reports UP.
- Grafana dashboard displays application metrics.
- Spring Boot Actuator health endpoint reports UP.

## GitHub Actions Workflow

Repository:

https://github.com/Mayor441/automated-cicd-delivery-platform

The GitHub Actions workflow is maintained at:

`.github/workflows/ci.yml`

The pipeline automatically executes when changes are pushed to the configured branch.

## Future Enhancements

- Add automated integration and end-to-end testing.
- Introduce centralized application logging.
- Add alerting through Prometheus Alertmanager.
- Implement secure authentication and authorization.
- Introduce Kubernetes-based deployment.
- Add infrastructure provisioning using Terraform.
- Improve deployment rollback and release strategies.

## Author

Developed as a practical DevOps and CI/CD portfolio project demonstrating Java application development, containerization, automated delivery, deployment, and monitoring.