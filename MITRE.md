MITRE is a ***not-for-profit organization*** that ***manages the CVE*** (Common Vulnerabilities and Exposures) Program. Its primary responsibility is to ***assign and maintain unique CVE identifiers*** and publish standardized vulnerability records, ensuring that the same ***vulnerability is consistently referenced across the cybersecurity industry***. 



In our project, MITRE ***served as the authoritative source*** for the base vulnerability information, including the CVE ID, description, publication date, last modification date, and reference links. We then ***enriched this information*** using NVD for CVSS scores and affected products, EPSS for exploitation probability, CISA KEV for known exploited vulnerabilities, ExploitDB for public exploits, and GitHub for Proof-of-Concept repositories before storing the final enriched record in OpenSearch.

