***# What security measures did you implement in your project?***

We adopted a layered security approach. During development, we integrated Bandit as our Static Application Security Testing (SAST) tool to identify insecure Python coding practices. We also integrated OWASP ZAP as our Dynamic Application Security Testing (DAST) tool to scan the running FastAPI application for web security vulnerabilities before deployment. In addition, we used Docker container isolation, AWS Security Groups, SSH key-based authentication, environment variables for sensitive configuration, and GitHub Secrets for CI/CD credentials.



***# Workflow:***

Developer Pushes Code

&#x20;       ▼

GitHub Actions Pipeline

&#x20;       ▼

Bandit (SAST)

&#x20;       ▼

Build Docker Image

&#x20;       ▼

Deploy to Test Environment

&#x20;       ▼

OWASP ZAP (DAST)

&#x20;       ▼

Security Report Generated

&#x20;       ▼

High Severity Issues?

&#x20;     ┌───────────────┐

&#x20;    Yes             No

&#x20;     │               │

&#x20;Stop Deployment   Deploy





***# SAST (Static Application Security Testing)***:

A security testing technique that analyzes the application's source code without executing it. It helps identify insecure coding practices, potential vulnerabilities, and security issues early in the development lifecycle before the application is deployed.



***# Which SAST tool did you use?***

We used Bandit, which is a Python Static Application Security Testing tool. It scans Python source code for common security issues based on predefined security rules and reports potential vulnerabilities that developers should review.



***# Why did you choose Bandit?***

Since our backend is developed in Python, Bandit was a suitable choice because it is lightweight, open-source, easy to integrate into CI/CD pipelines, and specifically designed to detect common Python security issues.



***# At what stage did you use Bandit?***

We integrated Bandit during the development and CI process. Before building and deploying Docker images, the source code was scanned using Bandit so that potential security issues could be identified and addressed early.



***# What types of vulnerabilities can Bandit detect?***

Bandit can detect several common Python security issues, including:

1. Hardcoded passwords or secrets
2. Use of insecure functions such as eval() or exec()
3. Weak cryptographic algorithms
4. Unsafe deserialization
5. Insecure temporary file usage
6. Insecure subprocess execution
7. Poor file permission handling

It helps developers identify insecure coding patterns before deployment.



***# Alternative to Bandit for Python SAST?***

Semgrep, GitHub CodeQL (Best for Deep Semantic Analysis), Pysa, Snyk (Commercial Tool)





***# OWASP ZAP***

OWASP ZAP (Zed Attack Proxy) is an open-source Dynamic Application Security Testing (DAST) tool developed by OWASP. It scans a running web application or REST API to identify runtime security vulnerabilities such as missing security headers, information disclosure, insecure cookies, XSS, SQL Injection, and other web security issues.



***# Why did you use OWASP ZAP?***

I wanted to automate security testing after deployment. Since UniVulner exposes REST APIs through FastAPI, I integrated OWASP ZAP into the CI/CD pipeline so every deployment automatically triggers a DAST scan and generates security reports without manual intervention.



***# Explain your DAST pipeline.***

After deployment to AWS EC2, GitHub Actions waits until the FastAPI application is available. Then it launches an OWASP ZAP Docker container, scans the application, generates HTML and JSON reports, parses the JSON report to summarize findings, and uploads both reports as GitHub Actions artifacts for developer review.



***# What reports does ZAP generate?***

I generate two reports:

HTML report for developers to review in a browser.

JSON report for automated parsing and processing.



***# Why parse the JSON report?***

Parsing the JSON report allows me to automatically summarize the scan results, such as the number of High, Medium, Low, and Informational findings. This makes the pipeline output easier to review without manually opening the HTML report.



***# Why use GitHub Actions artifacts?***

GitHub Actions artifacts provide a convenient way to preserve the generated reports after the workflow completes. Developers can download the HTML and JSON reports for review without accessing the EC2 server.



***# What vulnerability did you find using DAST?***

During DAST testing with OWASP ZAP, I identified a High-risk Server Side Include (SSI) Injection issue in my /export API. ZAP injected a malicious payload into the query parameter to check whether the application executed server-side include directives.



***# What is Server Side Include (SSI)?***

Server Side Include (SSI) is a feature used by web servers to dynamically include content in web pages. If user input is not properly validated, an attacker may inject SSI commands that the server could execute, potentially leading to information disclosure or even remote code execution.



***# How did you mitigate it?***

I reviewed the /export API to ensure that user-supplied input is never interpreted as server-side commands. I validated and sanitized the query parameter, treated it as plain text, avoided passing user input to shell commands or template engines, and returned properly encoded output. After applying the fix, I reran the ZAP scan to verify the issue was resolved.



| --------------------------------- | ------------------------------------------- |

| SAST (Bandit)                     | DAST (OWASP ZAP)                            |

| --------------------------------- | ------------------------------------------- |

| Analyzes source code              | Tests the running application               |

| White-box testing                 | Black-box testing                           |

| Runs during development           | Runs after deployment to a test environment |

| Detects insecure coding practices | Detects runtime web vulnerabilities         |

| Faster feedback                   | Simulates real-world attacks                |

| --------------------------------- | ------------------------------------------- |

