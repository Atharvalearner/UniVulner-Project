One of the practical challenges we faced was during the initial data ingestion phase. At first, our Initial Loader collected historical vulnerability data directly through multiple APIs such as MITRE, NVD, and other threat intelligence sources. However, when importing a large volume of historical data, we encountered ***API rate limits and long synchronization times***. Fetching thousands of records through APIs was not efficient.



To solve this, we changed our approach. Wherever the data providers offered official bulk data feeds, such as JSON feeds or downloadable datasets, we used those for the initial historical import. We processed those datasets, normalized and enriched the information, and then indexed it into OpenSearch.



After the initial bulk load was completed, the Scheduler switched to incremental API synchronization to fetch only newly published or modified vulnerabilities. This significantly reduced API requests, avoided rate-limiting issues, and made the synchronization process much more efficient.

