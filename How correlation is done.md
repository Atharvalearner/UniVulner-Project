The main objective is to avoid storing duplicate vulnerability records from different sources. Instead, I correlate all available information into a single unified vulnerability record using the CVE ID as the primary correlation key.



Step 1: The collectors fetch vulnerability information from multiple sources such as MITRE, NVD, EPSS, KEV, ExploitDB, and GitHub.



Step 2: Since every source returns data in a different JSON format, each collector normalizes its response into a common VulnerabilityRecord schema.



Step 3: The ***Aggregator merges the normalized records using the CVE ID.*** Instead of creating separate entries, it enriches the same vulnerability with additional information from different sources.



Step 4: The final correlated record contains the vulnerability description, CVSS score, EPSS score, KEV status, exploit references, GitHub PoCs, and other metadata in a ***single document***.



Step 5: This unified record is then ***indexed into OpenSearch***, allowing analysts to retrieve complete vulnerability information with a single search.





&#x20;                CVE-2021-44228

&#x20;                      │

&#x20;     ┌────────────────┼─────────────────┐

&#x20;     ▼                ▼                 ▼

&#x20;  MITRE             NVD              ExploitDB

&#x20;     │                │                 │

&#x20;     ▼                ▼                 ▼

&#x20;Normalize        Normalize         Normalize

&#x20;     │                │                 │

&#x20;     └──────────┬─────┴─────────────────┘

&#x20;                ▼

&#x20;       Vulnerability Aggregator

&#x20;                │

&#x20;       Merge using CVE ID

&#x20;                ▼

&#x20;       Stored in Model: VulnerabilityRecord

&#x20;                │

&#x20;    ┌───────────┼────────────┐

&#x20;    ▼           ▼            ▼

&#x20;  CVSS        EPSS         GitHub PoCs

&#x20;    │           │            │

&#x20;    └───────────┼────────────┘

&#x20;                ▼

&#x20;           OpenSearch







***# What if one source doesn't have data for a CVE?***

The correlation process is source-independent. If one source doesn't provide information for a CVE, the record is still created using the available sources. As new information becomes available in future synchronizations, the existing record is enriched rather than recreated.

