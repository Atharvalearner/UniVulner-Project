The Threat Score is calculated after all vulnerability data has been aggregated and enriched. Instead of relying on a single metric like CVSS, I use a weighted scoring model that combines multiple threat intelligence indicators to produce a score between 0 and 100.



Step 1: The platform first aggregates vulnerability data from multiple sources, including NVD, EPSS, CISA KEV, ExploitDB, and the GitHub PoC Correlation Engine.



Step 2: Each source contributes a specific risk indicator. For example, NVD provides the CVSS score, EPSS provides exploit probability, KEV indicates whether the vulnerability is actively exploited, ExploitDB confirms exploit availability, and GitHub provides the number of related PoC repositories.



Step 3: The ThreatScoreCalculator assigns predefined weights to each indicator and combines them into a final score.



Step 4: The calculated threat score is stored with the vulnerability record and is used to prioritize vulnerabilities for remediation.







&#x20;          Vulnerability Record

&#x20;                   │

&#x20;     ┌──────────────────────────┐

&#x20;     │       CVSS Score         │

&#x20;     │       EPSS Score         │

&#x20;     │       KEV Status         │

&#x20;     │   Exploit Availability   │

&#x20;     │ GitHub Repository Count  │

&#x20;     └──────────────────────────┘

&#x20;                   │

&#x20;       ThreatScoreCalculator

&#x20;                   │

&#x20;        Apply Weighted Rules

&#x20;                   │

&#x20;      Final Threat Score (0–100)

&#x20;                   │

&#x20;     Prioritize Vulnerabilities





**| ----------- | ------------------------------- | ------ |**

**| Factor      | Purpose                         | Weight |**

**| ----------- | ------------------------------- | ------ |**

| CVSS Score  | Technical severity              | 50 	 |

| EPSS Score  | Probability of exploitation     | 25 	 |

| KEV Status  | Actively exploited by attackers | 15 	 |

| ExploitDB   | Public exploit available        | 5  	 |

| GitHub PoCs | Number of related repositories  | 5  	 |

| ----------- | ------------------------------- | ------ |



***# Why these factors?***

Each factor represents a different aspect of risk. CVSS measures technical severity, EPSS estimates the likelihood of exploitation, KEV confirms active exploitation, ExploitDB indicates that exploit code is publicly available, and GitHub PoCs provide additional evidence of exploit activity. Combining these factors provides a more practical prioritization than using CVSS alone.

