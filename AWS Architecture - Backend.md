***# End-to-End Architecture:***



&#x20;                   Developer

&#x20;                        │

&#x20;                   Git Push

&#x20;                        │

&#x20;                   GitHub Actions

&#x20;                        │

&#x20;                   Build Docker Images

&#x20;                        │

&#x20;                   GHCR store docker images

&#x20;                        │

&#x20;                  docker compose pull

&#x20;                        │

&#x20;                  Ubuntu EC2 Instance

&#x20;                        │

&#x20;                  Docker Engine

&#x20;                        │

&#x20;    ┌──────────┬─────────────┬─────────────┐

&#x20;    ▼          ▼             ▼             ▼

&#x20;FastAPI   Scheduler   OpenSearch   Dashboards

&#x20;    └──────────┴─────────────┘─────────────┘

&#x20;               Docker Network





***# Why did you choose EC2?***

EC2 acts as the server that hosts the complete UniVulner platform. Instead of installing everything directly on Windows/Linux, we deploy everything inside Docker containers on Ubuntu EC2.



***# Why did you choose EC2 instead of Lambda?***

My application consists of long-running services like FastAPI, OpenSearch, and a Scheduler. AWS Lambda is designed for short-lived event-driven functions, whereas EC2 provides a continuously running server suitable for containerized applications.



***# Why did you use EC2?***

Because my application consists of continuously running Docker containers that require persistent compute resources and storage.



***# Why didn't you use Kubernetes?***

For this project, a single EC2 instance running Docker Compose was sufficient. Kubernetes would add operational complexity without significant benefits for a small-scale deployment. If the application needed to scale across multiple nodes or required advanced orchestration, Kubernetes would be a suitable next step.

