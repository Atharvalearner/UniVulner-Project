We deployed the project on an AWS EC2 instance using Docker Compose. Each component—React frontend, FastAPI backend, Scheduler, OpenSearch, and OpenSearch Dashboards—runs in its own container. The infrastructure is provisioned using Terraform, while GitHub Actions automates the CI/CD pipeline by building Docker images, running security scans, and pushing images to GitHub Container Registry. The EC2 instance pulls the latest images and starts all services through Docker Compose. We also configured persistent OpenSearch storage and used an Elastic IP to provide a stable public endpoint.





***# Simple Workflow:***

Developer

&#x20;    │

&#x20;    ▼

GitHub Repository

&#x20;    │

&#x20;    ▼

GitHub Actions (CI/CD)

&#x20;    ├── Build Docker Images

&#x20;    ├── Run Bandit (SAST)

&#x20;    ├── Run OWASP ZAP (DAST)

&#x20;    └── Push Images to GHCR

&#x20;             │

&#x20;             ▼

&#x20;       AWS EC2 Instance

&#x20;             │

&#x20;     Docker Compose

&#x20;┌────────────┼────────────┐

&#x20;▼            ▼            ▼

React      FastAPI     Scheduler

&#x20;             │

&#x20;             ▼

&#x20;        OpenSearch

&#x20;             │

&#x20;             ▼

&#x20;     OpenSearch Dashboards

