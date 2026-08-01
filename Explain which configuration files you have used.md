***1. Docker Compose Configuration (docker-compose.yml)***

It defines all the services required by the application, including the React frontend, FastAPI backend, Scheduler, OpenSearch, and OpenSearch Dashboards. It also specifies Docker networks, persistent volumes, port mappings, restart policies, and environment variables, allowing the complete application to be deployed using a single command.



***2. Dockerfiles (We used 3 files - initial loader, scheduler, fastapi)***

Each custom service has its own Dockerfile. The Dockerfile defines how the Docker image is built, including the base image, application dependencies, source code, exposed ports, and the startup command. This ensures that every service runs in a consistent environment regardless of the deployment platform.



***3. Environment Configuration (.env)***

We used environment variables to store configurable values such as OpenSearch connection details, API URLs, application settings, and other runtime configurations. This separates configuration from the application code and makes deployment easier across different environments.



***4. Terraform Configuration Files:***

provider.tf – Configures the AWS provider.

variables.tf – Defines configurable infrastructure parameters.

locals.tf – Stores reusable local values.

data.tf – Retrieves existing AWS resources, such as the latest Ubuntu AMI.

keypair.tf – Creates the EC2 SSH key pair.

security\_group.tf – Defines inbound and outbound firewall rules.

ec2.tf – Creates and configures the EC2 instance.

outputs.tf – Displays useful deployment outputs such as the EC2 public IP.

user\_data.sh – Automatically installs Docker and Docker Compose during EC2 initialization.



***5. GitHub Actions Workflow (.github/workflows/\*.yml):***

We used GitHub Actions workflow files to automate the CI/CD pipeline. The workflow builds Docker images, performs security scanning using Bandit and OWASP ZAP, pushes the images to GitHub Container Registry, and prepares them for deployment.



***6. Application Configuration***

FastAPI uses configuration files to define API settings, OpenSearch connection details, scheduler intervals, and logging configurations. These settings allow the application to be modified without changing the source code.





***# Summary:***

**| ----------------------- | ------------------------------------------------------ |**

**| Configuration File      | Purpose                                                |**

**| ----------------------- | ------------------------------------------------------ |**

| docker-compose.yml      | Orchestrates all application containers                |

| Dockerfile              | Builds Docker images for each service                  |

| .env                    | Stores runtime configuration and environment variables |

| provider.tf             | Configures AWS provider                                |

| variables.tf            | Defines Terraform input variables                      |

| locals.tf               | Stores reusable local values                           |

| data.tf                 | Retrieves existing AWS resources                       |

| keypair.tf              | Creates EC2 SSH key pair                               |

| security\_group.tf       | Configures AWS firewall rules                          |

| ec2.tf                  | Provisions EC2 instance                                |

| outputs.tf              | Displays deployment outputs                            |

| user\_data.sh            | Installs Docker during EC2 startup                     |

| .github/workflows/\*.yml | Defines the CI/CD pipeline                             |

| ----------------------- | ------------------------------------------------------ |



