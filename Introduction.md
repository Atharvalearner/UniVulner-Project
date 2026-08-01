***UniVulner***: ***self-hosted Vulnerability Intelligence*** *and **Organizational Risk Assessment Platform**.*



The primary ***objective*** of the project is to ***help organizations identify which vulnerabilities*** actually affect their software inventory and ***prioritize remediation*** based on multiple threat intelligence sources.



***# Problem:***

Traditional vulnerability intelligence platforms provide information about millions of CVEs, but organizations often struggle to determine which vulnerabilities are relevant to their own infrastructure.



To solve this problem, our platform first ***collects and continuously synchronizes*** vulnerability intelligence from multiple public sources such as MITRE, NVD, EPSS, CISA KEV, ExploitDB, and GitHub Proof-of-Concept repositories.



All of this ***heterogeneous data is normalized*** into a unified vulnerability model and indexed in OpenSearch.



The platform also accepts an organization's ***Software Bill of Materials (SBOM) or software inventory, correlates*** installed products and package versions with known vulnerabilities, ***enriches the matched vulnerabilities*** using threat intelligence, calculates a contextual priority score, and finally generates a ***prioritized assessment report***.



The application is ***deployed using Docker containers***, **automated** using ***GitHub Actions CI/CD***, and the cloud infrastructure is provisioned using ***Terraform on AWS***.



***# My-Contribution:***

My primary contribution was designing and implementing the data ingestion pipeline, normalization process, enrichment scheduler, OpenSearch integration, GitHub PoC correlation engine, SBOM processing workflow, and deployment automation.

