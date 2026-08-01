The objective of the GitHub PoC Correlation Engine is to automatically identify and prioritize the most relevant GitHub repositories for a given CVE instead of relying on a simple keyword search.



**Step 1:** When a CVE ID is received, the engine queries the GitHub Search API to **retrieve** the top matching repositories.



**Step 2**: Since the same repository can appear through different search results, I **remove duplicate** repositories using the Deduplication Engine.



**Step 3**: Each repository is then **enriched** by collecting additional metadata. Based on the repository name and description, I **classify it as a PoC, Exploit, or Scanner**.



**Step 4**: Next, every repository is assigned a **custom relevance score** using multiple quality factors such as CVE match, repository type, popularity, recent activity, and README quality.



**Step 5**: After scoring, all repositories are **ranked** in descending order of relevance, so the highest-confidence repositories appear first.



**Step 6**: Finally, the Alias Builder extracts product aliases from the selected repositories, and the Correlation Engine links the highest-ranked repositories with the corresponding CVE to build a unified vulnerability record.





***# Complete Flow:***

&#x20;             *CVE ID*

&#x20;                *▼*

&#x20;     *GitHub Search API*

&#x20;                *▼*

&#x20;     *Retrieve Top Repositories*

&#x20;                *▼*

&#x20;     *Remove Duplicates*

&#x20;                *▼*

&#x20;     *Enrich Repository Enrichment*

&#x20;                *▼*

&#x20;     *Repository Classification (PoC, Exploit, Scanner, Reference)*

&#x20;                *▼*

&#x20;     *Calculate* *Relevance Score (Repository name, README, Stars, Forks, Description, Keywords, Repository type*)

&#x20;                *▼*

&#x20;     *Ranking repositories*

&#x20;                *▼*

&#x20;     *Alias Generation* 

&#x20;                *▼*

&#x20;     *Correlate with CVE*

&#x20;                *▼*

&#x20;     *Structured GitHub Record Stored in OpenSearch*





***# On what basis do you calculate the repository score?:***

**| ----------------------- | ------------------------------------------------------------------------ |**

**| Factor                  | Purpose                                                                  |**

**| ----------------------- | ------------------------------------------------------------------------ |**

| Exact CVE Match         | Highest confidence that the repository targets the requested CVE **(+50)**   |

| Detected CVE References | Additional evidence that the repository is related to the CVE **(+30)**      |

| Repository Type         | **Exploit (+35), PoC (+30), Scanner (+15)**                                  |

| Stars                   | Indicates community popularity **(up to +20)**                               |

| Forks                   | Indicates community adoption **(up to +10)**                                 |

| Recently Updated        | Active repositories are preferred **(+10 or +5)**                            |

| README Quality          | Better documentation increases confidence **(up to +25)**                    |

| Generic Repositories    | **Repositories like "awesome", "list", or "collection" are penalized (-40)** |

**| ----------------------- | ------------------------------------------------------------------------ |**



