***# GitHub Actions***

GitHub's built-in CI/CD platform that automates software development workflows such as building, testing, packaging, and deploying applications in response to repository events.



***# GitHub Container Registry (GHCR):***

GHCR integrates directly with GitHub, securely stores Docker images, supports private repositories, and works seamlessly with GitHub Actions for automated image publishing.

Instead of storing Docker images on your laptop, you store them in GitHub Container Registry.



***# GitHub runner:***

A virtual machine or application that executes jobs in a GitHub Actions workflow.

There are two main types: GitHub-hosted runners and self-hosted runners.

We used GitHub-hosted runners which is managed by GitHub.



***# Advantages:***

Integrated with GitHub

Supports private repositories

No need for another account

Uses GitHub authentication

Easy integration with GitHub Actions



***# Components of Your GitHub Action***

1\. Trigger: Starts when code is pushed.

2\. Checkout: Downloads the repository into the GitHub runner.

3\. Login: Authenticates to GitHub Container Registry.

4\. Build: Creates Docker images.

5\. Push: Uploads images to GHCR.



***# Deployment Architecture With GitHub Actions:***

&#x20;                Developer

&#x20;                    │

&#x20;               Git Push

&#x20;                    ▼

&#x20;           GitHub Repository

&#x20;                    ▼

&#x20;           GitHub Actions (CI)

&#x20;     ┌─────────────────────────────┐

&#x20;     │ • Checkout Code             │

&#x20;     │ • Build Docker Images       │

&#x20;     │ • Login to GHCR             │

&#x20;     │ • Push Images               │

&#x20;     └─────────────────────────────┘       │

&#x20;                    ▼

&#x20;     GitHub Container Registry (GHCR)

&#x20;                    │

&#x20;             docker compose pull

&#x20;                    │

&#x20;              AWS EC2 Instance

&#x20;                    │

&#x20;                    ▼

&#x20;     Docker Compose Starts Containers

&#x20;                    │

&#x20;     ┌──────────┬────────────┬─────────────┐

&#x20; FastAPI   Scheduler   OpenSearch      UniVulner





***# What happens after you push code?***

After pushing code to GitHub, GitHub Actions starts the workflow, checks out the repository, builds the Docker images, authenticates with GHCR, pushes the images to the registry, and makes them available for deployment.



***# Where are GitHub Action workflows stored?***

They are stored in the repository under the ***.github/workflows/*** directory as YAML files.



**# How many stages are there in your project github actions ci/cd pipeline?** 

Our GitHub Actions pipeline is divided into two workflows:

* CI (Continuous Integration) – responsible for code quality, testing, security checks, infrastructure validation, and Docker image generation.
* CD (Continuous Deployment) – responsible for building production images, publishing them to GitHub Container Registry, deploying them on AWS EC2, verifying the deployment, and performing Dynamic Application Security Testing (DAST).





Developer Push

&#x20;       ▼

─────────────── CI ─────────────── (6 Stages)

Checkout		.. Fetches the latest source code from GitHub so the pipeline can execute all subsequent tasks.

&#x20;       │

Python Setup		.. Installs Python 3.11 and all project dependencies required for testing and analysis.

&#x20;       │

Black + Flake8		.. We first verify code formatting using Black, then perform static code quality analysis using Flake8 to identify syntax issues, style violations, and potential coding mistakes.

&#x20;       │

Bandit (SAST)		.. Performs static security analysis of the Python source code to identify insecure coding practices and potential security vulnerabilities.

&#x20;       │

Terraform Validation	.. Checks that the Terraform configuration is properly formatted and syntactically valid before infrastructure deployment.

&#x20;       │

Docker Build		.. Builds Docker images for the FastAPI service and Scheduler service to verify that the application can be successfully containerized.



─────────────── CD ─────────────── (7 Stages)

Checkout			.. Fetch latest code.

&#x20;       │

Login GHCR \& Build Images	.. Authenticates with GitHub Container Registry and builds Docker images.

&#x20;       │

Push Images			.. Pushes the Docker images to GitHub Container Registry so they are available for deployment.

&#x20;       │

Deploy to EC2			.. Connects securely to the AWS EC2 instance using SSH, pulls the latest Docker images from GHCR, and updates the running containers using Docker Compose.

&#x20;       │

Application Health Check	.. Verifies that the application has started successfully by checking the FastAPI OpenAPI endpoint before proceeding.

&#x20;       │

OWASP ZAP (DAST)		.. Executes an automated OWASP ZAP API scan against the deployed FastAPI application to identify runtime security vulnerabilities.

&#x20;       │

Upload Security Reports		.. Parses the OWASP ZAP report into a readable format and uploads the generated security reports as GitHub workflow artifacts.

