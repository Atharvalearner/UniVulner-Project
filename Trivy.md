Trivy is an open-source security scanner that we integrated into our CI/CD pipeline to identify security vulnerabilities before deployment. In our project, its primary role was to scan the Docker images generated during the build process.



Trivy checks the operating system packages and application dependencies inside the Docker image against known vulnerability databases and reports vulnerabilities such as Critical, High, Medium, and Low severity.



By integrating Trivy into GitHub Actions, we ensured that container images were scanned automatically before deployment, helping us detect vulnerable packages early and improving the overall security of the application.



**# Simple Answer:**

We used Trivy for container image vulnerability scanning. It automatically scans Docker images during the CI/CD pipeline and reports known vulnerabilities before deployment.



**# Where is Trivy used in your pipeline?**

Developer Push

&#x20;      │

GitHub Actions

&#x20;      ├── Checkout Source Code

&#x20;      ├── Build Docker Images

&#x20;      ├── Bandit (SAST)

&#x20;      ├── Trivy Image Scan

&#x20;      ├── OWASP ZAP (DAST)

&#x20;      ├── Push Images to GHCR

&#x20;      ▼

AWS EC2 Deployment



**# What if Trivy finds vulnerabilities?**

If Trivy reports High or Critical vulnerabilities, we first identify the affected package from the report. If a patched version is available, we update the base image or application dependency and rebuild the Docker image. The image is rescanned until the critical issues are resolved before deployment.

