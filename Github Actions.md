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

