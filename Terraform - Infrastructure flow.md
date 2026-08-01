***# Workflow/Architecture:***



&#x20;                Terraform

&#x20;    ┌───────────────┼────────────────┐

provider.tf     variables.tf     locals.tf

&#x20;    │               │                │

&#x20;    └───────────────┼────────────────┘

&#x20;                 data.tf

&#x20;                    │

&#x20;         Find Latest Ubuntu AMI

&#x20;                    │

&#x20;                    ▼

&#x20;               keypair.tf

&#x20;         Generate SSH Key Pair

&#x20;                    │

&#x20;                    ▼

&#x20;          security\_group.tf

&#x20;         Configure Firewall Rules

&#x20;                    │

&#x20;                    ▼

&#x20;                ec2.tf

&#x20;         Launch EC2 Instance

&#x20;                    │

&#x20;                    ▼

&#x20;             user\_data.sh

&#x20;    Install Docker \& Configure Server

&#x20;                    │

&#x20;                    ▼

&#x20;               outputs.tf

&#x20;     Display Public IP, DNS, SSH Info





***1. Terraform Provider:***

* A provider is a plugin that enables Terraform to communicate with a specific platform such as AWS, Azure, Docker, or Kubernetes.
* Here we need AWS provider Because Terraform itself cannot create AWS resources. The AWS provider translates Terraform configuration into AWS API calls from that it will manage all resources.
* **AWS Provider** → Creates AWS resources.
* **TLS Provider** → Generates SSH key pairs.
* **Local Provider** → Saves files (like the private key) on your local machine.
* Terraform downloads providers during ***terraform init***





***2. Terraform Variables:***

* Stores configurable values, We define them once and used throughout the project.
* It improve flexibility and reusability by allowing the same Terraform configuration to be used across multiple environments without changing the source code.
* eg. We create Variables like aws\_region, instance\_type, instance\_name, private\_key\_path





***3. Terraform Data Source:***

* A data source retrieves existing information from the cloud.
* Example: AMI IDs keep changing, So instead of hardcoding it Terraform searches AWS and automatically finds the latest Ubuntu image.





***4. Terraform Locals:***

* Stores values that are reused within the Terraform configuration.
* eg. We used in our project: Project="UniVulner", ManagedBy="Terraform"





***5. Terraform Keypair:***

* Automatically generate SSH keys and register them with AWS.
* Workflow: *Terraform >> Generate Private Key >> Generate Public Key >> Upload Public Key to AWS >> Save Private Key Locally >> SSH into EC2*





***6. Terraform Security Group:***

* Stateful virtual firewall that controls inbound and outbound traffic for AWS resources such as EC2 instances.
* Acts as the firewall for your EC2 instance.
* Controls who can connect, which ports are open





***7. Terraform Resources:***

* Representing any infrastructure object (EC2, database, DNS, Network, etc) you want to create, configure, and manage.
* In our project we Creates EC2 instance, aws key pair, etc





***8. Terraform Outputs:***

* Displays useful information after deployment. eg. IP addresses or instance IDs, Public DNS, SSH Command
* expose important information about the deployed infrastructure, so it can be easily referenced after deployment or by other Terraform configurations.







***# Difference between Variables and Locals?***

Variables are input values that can change between environments, while locals are internal values computed or reused within the Terraform configuration.



***# Why only upload the public key?***

The public key is used by AWS to verify incoming SSH connections, while the private key remains securely on the client's machine for authentication.



***# Why generate keys using Terraform?***

It automates the infrastructure provisioning process and eliminates the need to manually create and upload SSH key pairs.



***# What happens when you run terraform apply?***

Terraform compares the desired state with the current state, creates or updates the required resources, and records them in the state file.



***# How does Terraform know what it has already created?***

It tracks managed resources in the Terraform state file (terraform.tfstate), which maps your configuration to the real infrastructure.

