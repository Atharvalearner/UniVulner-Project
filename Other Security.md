***# How did you protect your application against SQL Injection?***

Our backend primarily interacts with OpenSearch rather than executing raw SQL queries. We avoid dynamically constructing database queries from user input. User input is validated before being used in search requests, reducing the risk of injection attacks. Additionally, SAST using Bandit and DAST using OWASP ZAP help identify potential injection-related issues during development and testing.



***# How did you prevent Cross-Site Scripting (XSS)?***

Our project is primarily a REST API built with FastAPI, so it doesn't directly render user-generated HTML in the browser. During security testing, OWASP ZAP was used to identify common web vulnerabilities, including reflected XSS. For a production deployment, I would also implement Content Security Policy (CSP), output encoding, and input validation wherever user-generated content is displayed.



***# How did you secure your APIs?***

We secured our APIs by following secure coding practices, validating incoming requests, keeping sensitive configuration in environment variables instead of source code, and scanning the application using Bandit and OWASP ZAP.



***# How did you secure Docker containers?***

Each component of UniVulner—FastAPI, Scheduler, OpenSearch, and Dashboards—runs in a separate Docker container. This provides service isolation and limits the impact if one container is compromised. The containers communicate through an internal Docker network, reducing unnecessary exposure.



***# How did you secure your AWS deployment?***

We secured the AWS deployment using Security Groups to allow only required ports, SSH key-based authentication for EC2 access, environment variables for application configuration, and GitHub Secrets for deployment credentials.



***# Why did you use GitHub Secrets?***

GitHub Secrets securely store credentials required by the CI/CD pipeline, such as container registry tokens. This prevents sensitive information from being hardcoded into workflow files and keeps credentials encrypted within GitHub.



***# How would you further improve the security of your project?***

For a production deployment, I would implement JWT authentication, RBAC, HTTPS with TLS, AWS WAF, IAM roles for AWS resources, rate limiting, audit logging, dependency vulnerability scanning, OpenSearch Security Plugin, continuous monitoring, and regular penetration testing.



***# How did you implement rate limiting in your project? (using SlowAPI)***

We implemented API rate limiting using SlowAPI, which integrates with FastAPI. Each client IP is allowed a fixed number of requests within a specified time window. If a client exceeds the configured limit, the server returns an HTTP 429 Too Many Requests response. This helps protect the application from brute-force attacks, API abuse, and denial-of-service attempts.



***# How did you implement input validation? (using Pydantic model)***

We implemented input validation using Pydantic models provided by FastAPI. Every incoming API request is validated against predefined schemas before being processed. If the request contains missing fields, incorrect data types, or values that violate validation rules, FastAPI automatically returns an HTTP 422 validation error.

