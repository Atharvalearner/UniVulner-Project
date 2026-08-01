***# We implemented several advanced techniques throughout the project:***

**1. Multi-source Threat Intelligence Collection:**

Instead of relying on a single vulnerability feed, the platform continuously synchronizes data from multiple trusted sources including MITRE, NVD, EPSS, CISA KEV, ExploitDB, and GitHub Proof-of-Concept repositories.



**2. Data Normalization Pipeline**

Since every source exposes different APIs, JSON structures, and metadata, we designed a normalization layer that converts heterogeneous vulnerability data into a common internal VulnerabilityRecord schema.



**3. Threat Intelligence Enrichment**

After normalization, each vulnerability is enriched with additional intelligence such as CVSS metrics, EPSS exploit probability, KEV exploitation status, exploit references, and GitHub PoC repositories, creating a single enriched vulnerability record.



**4. SBOM-Based Vulnerability Correlation**

Organizations can upload an SBOM or software inventory. The platform extracts software components, vendors, products, and versions, normalizes them, and automatically correlates them with the vulnerability database to identify affected assets.



**5. Contextual Risk Prioritization**

Instead of displaying only vulnerability information, the platform combines vulnerability intelligence with the organization's software inventory to generate a prioritized assessment report. This helps organizations focus on vulnerabilities that actually impact their environment.



**6. Full-Text Search and Analytics**

All normalized vulnerability records are indexed in OpenSearch, enabling fast full-text search, filtering, aggregation, and analytics over a large volume of vulnerability data.



**7. Automated Synchronization**

We separated historical data ingestion from incremental updates. The Initial Loader performs the one-time historical import, while the Scheduler periodically synchronizes newly published or modified vulnerabilities, reducing synchronization time and API usage.



**8. DevSecOps \& Cloud Automation**

The application is containerized using Docker, infrastructure is provisioned using Terraform on AWS, CI/CD is implemented with GitHub Actions, and security testing is integrated using Bandit (SAST) and OWASP ZAP (DAST).





***# Why did you add SBOM?***

We introduced SBOM support because organizations are not interested in all published CVEs. They want to know which vulnerabilities actually affect the software they use. By correlating the uploaded SBOM with our vulnerability intelligence database, the platform automatically identifies affected components and generates a prioritized remediation report. This makes the platform more practical for vulnerability management.



***# What is the biggest innovation in your project?***

The biggest innovation is integrating SBOM-based software inventory analysis with multi-source vulnerability intelligence. Instead of presenting a generic list of vulnerabilities, the platform performs asset-aware correlation and produces a contextual, prioritized risk assessment for the organization

