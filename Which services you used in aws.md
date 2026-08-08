For deploying UniVulner, we primarily used Amazon EC2 to host the application. The complete infrastructure was provisioned using Terraform.



The AWS services we used include:

* **Amazon EC2** – to host the Dockerized application.
* **Amazon VPC** – the EC2 instance was deployed inside the default VPC for network isolation.
* **Security Groups** – to control inbound and outbound network access.
* **Elastic IP** – to provide a static public IP address for stable deployment.
* **EC2 Key Pair** – for secure SSH-based authentication.
* **Amazon EBS** – the root storage volume attached to the EC2 instance to store the operating system, Docker images, and OpenSearch data.



Terraform automated the provisioning of these resources, while Docker Compose deployed the application services on the EC2 instance.





**# Simply:**

We primarily used Amazon EC2 to host our Dockerized application. The EC2 instance was deployed inside an AWS VPC and protected using Security Groups, which allowed only the required ports such as SSH, FastAPI, OpenSearch, and Dashboards. We used an Elastic IP to provide a stable public endpoint, EC2 Key Pairs for secure SSH authentication, and Amazon EBS as persistent storage for the operating system, Docker images, and OpenSearch data. All of these resources were provisioned automatically using Terraform, while the application itself was deployed using Docker Compose.

