**# Why did you use FastAPI in your project?**

We used FastAPI as the backend framework because it provides a high-performance REST API framework, automatic request validation using Pydantic, automatic OpenAPI/Swagger documentation, and asynchronous request handling. Since our project exposes vulnerability search, SBOM assessment, and report generation as REST APIs, FastAPI was a good choice due to its speed, simplicity, and built-in API documentation.



**# Why do you need APIs in your project?**

Our project consists of multiple independent components such as OpenSearch, the Scheduler, the Threat Intelligence module, the SBOM Assessment module, and the Web Dashboard. Instead of allowing the frontend or users to access OpenSearch directly, we expose all functionality through REST APIs.



The API layer acts as the communication bridge between the frontend and the backend. It receives requests, validates them, executes the required business logic, queries OpenSearch, and returns structured JSON responses to the client.



Web Dashboard

&#x20;     │

&#x20;     ▼

&#x20;FastAPI REST APIs

&#x20;     │

&#x20;┌────┴───────────────┐

&#x20;│                    │

&#x20;▼                    ▼

Threat Intelligence   SBOM Module

&#x20;│                    │

&#x20;└──────────┬─────────┘

&#x20;           ▼

&#x20;     OpenSearch





**# How does FastAPI work in your project?**

***I) Suppose a user searches for CVE-2021-44228.***

1. The user enters the CVE ID on the web dashboard.
2. The frontend sends an HTTP GET request to the FastAPI endpoint.
3. FastAPI validates the request.
4. FastAPI queries OpenSearch.
5. OpenSearch returns the normalized and enriched vulnerability record.
6. FastAPI converts it into a JSON response.
7. The frontend displays the vulnerability details to the user.



***II) For SBOM Assessment***

1. When an organization uploads a software inventory CSV:
2. The frontend uploads the file to FastAPI.
3. FastAPI validates the uploaded file.
4. The SBOM module extracts vendor, product, and version information.
5. The correlation engine searches OpenSearch for matching vulnerabilities.
6. The threat score is calculated.
7. FastAPI returns the prioritized vulnerability assessment report.





**# What APIs did you expose?**

* Searching vulnerabilities by CVE ID or keyword.
* Retrieving detailed vulnerability information.
* Filtering vulnerabilities.
* Uploading SBOM/software inventory.
* Performing vulnerability correlation.
* Generating the prioritized assessment report.



**# What type of APIs did you use?**

We implemented RESTful APIs that exchange data in JSON format using standard HTTP methods such as GET and POST.

