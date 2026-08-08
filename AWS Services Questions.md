**# Why didn't you use AWS ELK/OpenSearch Service?**

We evaluated both self-managed OpenSearch and the AWS managed OpenSearch Service. Since UniVulner is a self-hosted prototype developed for learning and demonstrating the complete deployment lifecycle, we ***chose a self-managed OpenSearch instance running in Docker.***



This gave us ***complete control over configuration, indexing, backups, plugins, and deploymen***t while keeping the infrastructure simple and cost-effective. It also allowed us to understand how OpenSearch is installed, configured, and managed instead of relying on a managed cloud service.



For an enterprise production deployment, I would consider AWS OpenSearch Service because it provides managed scaling, automated backups, monitoring, high availability, and reduced operational overhead.







**# Why didn't you use Kubernetes instead of Docker Compose?**

Kubernetes is an excellent orchestration platform for large-scale production environments. However, our application consists of only a few services running on a single EC2 instance, so Docker Compose was sufficient for our requirements.



Docker Compose allowed us to deploy and manage all containers with minimal operational complexity while keeping the architecture easy to understand and maintain.



If the platform needed to support multiple nodes, high availability, auto-scaling, rolling updates, or self-healing, Kubernetes would be the appropriate choice.







**# Why not use ECS (Elastic Container Service) ?**

ECS is a managed container orchestration service that simplifies container deployment. However, because our project already uses Docker Compose on a single EC2 instance, ECS would not provide significant benefits for the current scale. For larger production deployments, ECS or Kubernetes would be appropriate options.







**# One-Line Comparison**

| Question                    | Best Answer                                                                                                   |

| --------------------------- | ------------------------------------------------------------------------------------------------------------- |

| Why Docker Compose?         | Single EC2, few services, low operational complexity                                                          |

| Why not Kubernetes?         | Kubernetes is better for large-scale orchestration; unnecessary for our current deployment                    |

| Why AWS OpenSearch Service? | Managed service is ideal for production, but we wanted full control and lower cost in a self-hosted prototype |

| Why AWS?                    | Mature ecosystem, Terraform integration, strong industry adoption                                             |

| Why not Azure/GCP?          | Architecture is cloud-agnostic; AWS was chosen based on project requirements and familiarity                  |





**# How much confidence you have over kubernetes and deploying a project on your own ?**

I am confident in deploying applications independently using Docker, Docker Compose, Terraform, AWS EC2, and GitHub Actions because I implemented and deployed UniVulner using these technologies.



Regarding Kubernetes, I have a good conceptual understanding of its architecture and core components such as Pods, Deployments, Services, ConfigMaps, Secrets, Ingress, and ReplicaSets. I understand when Kubernetes should be used and how it differs from Docker Compose.



However, I haven't deployed this project on a production Kubernetes cluster yet. If required, I am confident that I can do it because I already understand containerization, networking, volumes, and deployment concepts. The remaining part is learning Kubernetes manifests and orchestration, which I am actively improving.





**# Rate yourself out of 10 in Kubernetes.**

I would rate myself around 6.5 to 7 out of 10. I have a solid understanding of Kubernetes concepts and can read and understand manifests, but I still need more hands-on production experience. Since I already work comfortably with Docker, Docker Compose, AWS, and Terraform, I'm confident I can become productive with Kubernetes quickly.





**# Can you deploy your project on Kubernetes?**

Yes, I can. Since the application is already containerized, the first step would be to create Kubernetes manifests or Helm charts for each service, such as FastAPI, Scheduler, OpenSearch, and the frontend. I would define Deployments, Services, Persistent Volumes for OpenSearch, ConfigMaps and Secrets for configuration, and an Ingress for external access. While I haven't completed this migration yet, I understand the deployment workflow and am confident I can implement it.



**# Why did you use Terraform instead of creating resources manually?**

Creating infrastructure manually through the AWS Console is time-consuming and prone to human errors. Terraform allows us to define the entire infrastructure as code, so the same infrastructure can be recreated consistently at any time.

It also supports version control, making infrastructure changes easy to track and reproduce across environments.



**# What is Terraform State?**

Terraform maintains a state file (terraform.tfstate), which records the current state of the infrastructure it manages. During subsequent runs, Terraform compares the desired configuration with the state file to determine what needs to be created, modified, or deleted.

