&#x20;                        UniVulner

&#x20;     Self-Hosted Vulnerability Intelligence

&#x20;     with Organizational Risk Assessment Platform



&#x20;┌────────────────────────────────────────────────────────────┐

&#x20;│              Threat Intelligence Sources                   │

&#x20;│ MITRE │ NVD │ EPSS │ KEV │ ExploitDB │ GitHub PoCs │

&#x20;└────────────────────────────────────────────────────────────┘

&#x20;                          │

&#x20;                          ▼

&#x20;                 Data Collection Layer

&#x20;                          │

&#x20;                          ▼

&#x20;                 Data Normalization Layer

&#x20;                          │

&#x20;                          ▼

&#x20;                   Data Enrichment Layer

&#x20;                          │

&#x20;                          ▼

&#x20;                OpenSearch Vulnerability Index

&#x20;                          │

&#x20;         ┌────────────────┴─────────────────┐

&#x20;         │                                  │

&#x20;         ▼                                  ▼

&#x20;**Threat Intelligence Module**         **SBOM Assessment Module**

&#x20; • CVE Multi-Search                • SBOM Upload

&#x20; • Filtering                       • Component Extraction

&#x20; • Analytics                       • Inventory Normalization

&#x20; • Vulnerability Details           • CVE Correlation

&#x20;                                   • Risk Assessment

&#x20;                                   • Prioritized Report

&#x20;         └────────────────┬─────────────────┘

&#x20;                          ▼

&#x20;                   FastAPI REST APIs

&#x20;                          │

&#x20;                          ▼

&#x20;                   Web Dashboard / UI

&#x20;                          │

&#x20;                          ▼

&#x20;         Docker • GitHub Actions • Terraform • AWS





***# Theory Explanation:***

The architecture of UniVulner is designed as a modular data processing pipeline, where each layer has a specific responsibility. This makes the system easier to maintain, extend, and scale.



***1. Threat Intelligence Collection***

The platform continuously collects vulnerability intelligence from multiple public sources including: MITRE, NVD, EPSS, CISA KEV, ExploitDB, GitHub repositories



Each source has its own dedicated collector because every source exposes different APIs, formats, and metadata.



***2. Normalization Layer***

This allows the rest of the system to work with a consistent data model instead of handling multiple external formats.

Since every source returns different JSON structures and field names, the normalizers convert them into a common internal VulnerabilityRecord schema.



***3. Enrichment Layer:***

This creates a single enriched vulnerability record instead of relying on a single source.

combines additional information from different sources, such as: CVSS metrics, EPSS score, KEV status, Exploit references, GitHub PoC repositories



***4. Storage Layer:***

This forms the centralized vulnerability intelligence database used by the platform.

OpenSearch provides: Full-text search, Filtering, Aggregation, Fast retrieval, Analytics



***5. Search \& Intelligence Module:***

This provides centralized access to vulnerability information collected from multiple sources.

Users can:

Search CVEs

View vulnerability details

Filter results

Explore references

Analyze enriched threat intelligence



***6. SBOM Assessment Module:***

Organizations can upload an SBOM or software inventory.

The platform extracts: Software components, Vendors, Products, Package versions

These components are normalized and matched against the vulnerability database.



***7. Correlation \& Risk Assessment:***

The correlation engine identifies which vulnerabilities affect the uploaded software inventory.

For every matched vulnerability, the platform combines information such as:

CVSS, EPSS, KEV, Exploit references, GitHub PoC

to generate a prioritized vulnerability assessment report.



***8. API Layer:***

The processed data is exposed through FastAPI REST APIs, which provide:

Vulnerability search, CVE details, SBOM Integration, Assessment results, Report generation, Dashboards



***9: Deployment Layer:***

The application is containerized using Docker, deployed on AWS using Terraform, and automated through GitHub Actions CI/CD for consistent deployment and infrastructure management.

