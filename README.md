# ASP.NET Enterprise Solution

A comprehensive and scalable boilerplate solution for building enterprise-grade applications using ASP.NET Core. Designed with "big scale business" needs in mind, it establishes a robust multi-service architecture ready for complex, high-performance, and resilient systems.

---

## Table of Contents

* [Overview](#overview)
* [Architecture](#architecture)
* [Key Features & Technologies](#key-features--technologies)
* [Getting Started](#getting-started)

  * [Prerequisites](#prerequisites)
  * [Project Structure](#project-structure)
  * [Configuration](#configuration)
  * [Building and Running](#building-and-running)
  * [Accessing Services](#accessing-services)
* [Customization](#customization)
* [Monitoring with Prometheus & Grafana](#monitoring-with-prometheus--grafana)
* [Scaling & Production Considerations](#scaling--production-considerations)
* [Contributing](#contributing)
* [License](#license)

---

## Overview

This project provides a foundational setup for an enterprise-level web application. It demonstrates how to integrate various essential services using Docker Compose, creating an isolated and easily reproducible development environment.

The aim is to offer a robust starting point that addresses common requirements for large-scale business applications, including:

* Data management
* Caching
* Asynchronous messaging
* Traffic routing
* Observability

---

## Architecture

The solution is designed with a microservices-inspired approach, where each core function is a distinct, containerized service.

```
+----------------+
|                |
|  Internet/User |
|                |
+-------+--------+
        |
        | (HTTP/HTTPS)
        V
+---------------------+
|                     |
|  Nginx Reverse Proxy|
|    (proxy service)  |
+-------+-------------+
        |
        |  +-------------------+
        |  |                   |
        +--> Frontend Website  |
        |  | (website service) |
        |  +-------------------+
        |
        |  +-------------------+      +-----------------+
        |  |                   |      |                 |
        +--> ASP.NET Core API  +------> SQL Server      |
        |  | (webapi service)  |      | (sqlserver)     |
        |  +-------+-----------+      +-----------------+
        |          |
        |          |  +-----------------+
        |          |  |                 |
        |          +--> Redis Cache    |
        |          |  | (redis service) |
        |          |  +-----------------+
        |          |
        |          |  +-------------------+
        |          |  |                   |
        |          +--> RabbitMQ Broker   |
        |             | (rabbitmq service) |
        |             +-------------------+
        |
        |  +-------------------+      +-----------------+
        |  |                   |      |                 |
        +--> Prometheus Metrics+------> Grafana Dashboards|
           | (prometheus)       |      | (grafana)        |
           +-------------------+      +-----------------+
```

---

## Key Features & Technologies

* **ASP.NET Core 8.0**: High-performance backend framework
* **Docker & Docker Compose**: Environment consistency
* **Microsoft SQL Server**: Reliable relational DB
* **Redis**: Fast in-memory data store
* **RabbitMQ**: Robust message broker
* **Nginx**: Load balancer and reverse proxy
* **Prometheus**: Metrics collection
* **Grafana**: Visualization and dashboarding
* **Multi-stage Dockerfiles**: Optimized builds
* **Health Checks**: Service readiness
* **Named Volumes & Custom Networks**: Persistent and isolated service data

---

## Getting Started

### Prerequisites

* Docker Desktop: [Download](https://www.docker.com/products/docker-desktop)
* .NET SDK 8.0: [Download](https://dotnet.microsoft.com/download)

### Project Structure

```
aspnet-enterprise-solution/
├── docker-compose.yml
├── .env
├── webapi/
│   ├── Dockerfile
│   └── YourProjectName.csproj
├── nginx/
│   └── nginx.conf
├── website/
│   └── index.html
├── prometheus/
│   └── prometheus.yml
└── grafana/
    └── grafana.ini
```

### Configuration

1. Clone the repository:

```bash
git clone https://github.com/your-username/aspnet-enterprise-solution.git
cd aspnet-enterprise-solution
```

2. Create `.env` file:

```dotenv
SA_PASSWORD=YourStrongSQLServerPassword!123
```

3. Place your ASP.NET Core files in `webapi/` and update Dockerfile references.
4. Configure your ASP.NET Core app to expose Prometheus metrics (e.g., via Prometheus.Client.AspNetCore).

---

### Building and Running

```bash
docker compose up --build -d
```

### Accessing Services

* Frontend Website: [http://localhost](http://localhost)
* API (Swagger): [http://localhost/api/swagger](http://localhost/api/swagger)
* RabbitMQ: [http://localhost:15672](http://localhost:15672) (guest/guest)
* Prometheus: [http://localhost:9090](http://localhost:9090)
* Grafana: [http://localhost:3000](http://localhost:3000) (admin/admin)

### Managing Containers

```bash
docker compose ps              # Check container status
docker compose logs -f webapi  # Logs for webapi
docker compose down            # Stop services
docker compose down -v        # Stop and remove all data
```

---

## Customization

* **ASP.NET Core**: Replace `webapi` placeholder with your real project.
* **DB Migrations**: Integrate EF Core Migrations.
* **Nginx**: Edit `nginx.conf` for routing or HTTPS.
* **Frontend**: Replace `website/index.html` with your build output.
* **.env**: Add required config vars.

---

## Monitoring with Prometheus & Grafana

* **Prometheus** scrapes `/metrics` from the webapi.
* **Grafana** connects to Prometheus (`http://prometheus:9090`) and visualizes the metrics.
* Extend `prometheus.yml` to include additional metrics.
* Import dashboards into Grafana for deeper insights.

---

## Scaling & Production Considerations

### Use Container Orchestration

* **Kubernetes**: For large-scale, production-grade systems
* **Docker Swarm**: Lightweight alternative

### Additional Best Practices

* Secrets: Use Vault/KMS, not `.env`
* HTTPS: Use Let's Encrypt with Nginx
* Logging: Centralized logging (e.g., ELK, Loki)
* Load Balancing: Enable multi-instance deployments
* Security: Harden container images and configs
* CI/CD: Automate builds, tests, and deployment
* Backups: Enable persistent volume backups

---

## Contributing

We welcome contributions! Submit issues and pull requests to improve this project.

---

## License

This project is licensed under the MIT License. See `LICENSE` file for more details.
