**# How is OpenSearch used in your project?**

The primary requirement of my project is fast vulnerability search and analytics rather than transactional data processing. That's why I chose OpenSearch instead of a traditional relational database.



1\. After collecting and normalizing vulnerability data, every CVE is ***indexed as a document*** in OpenSearch.



2\. OpenSearch creates an ***inverted index***, which makes full-text searches on CVE IDs, descriptions, vendors, products, and references very fast.



3\. When a user searches for a vulnerability, ***OpenSearch searches the index instead of scanning the entire dataset***, resulting in low-latency responses.



4\. OpenSearch also ***supports filtering, aggregations, and analytics***, which are useful for dashboards such as severity distribution, vendor-wise vulnerabilities, and exploit statistics.





**# Why did you use OpenSearch in your project?**

OpenSearch serves as the centralized vulnerability intelligence database in our project. Since we continuously collect vulnerability data from multiple sources such as MITRE, NVD, EPSS, CISA KEV, ExploitDB, and GitHub, we needed a search engine capable of storing, indexing, and retrieving a large number of vulnerability records efficiently.



Instead of storing everything in a traditional relational database, we used OpenSearch because it provides high-performance full-text search, filtering, aggregations, and near real-time indexing, making it ideal for vulnerability intelligence platforms.





**# How does OpenSearch return only relevant records?**

Every vulnerability record is indexed based on searchable fields such as CVE ID, vendor, product, affected versions, severity, keywords, and references. When a user searches or uploads an SBOM, FastAPI constructs the query, and OpenSearch retrieves only the matching indexed records instead of scanning the complete dataset sequentially.





**# What is Indexing?**

Indexing is the process of storing data in a structure optimized for searching. Instead of performing a sequential scan over every vulnerability record, OpenSearch creates indexes on searchable fields, enabling fast retrieval of matching documents.





**# Is Opensearch dashboard Same as Kibana ?**

Yes. OpenSearch Dashboards, which is the open-source equivalent of Kibana, for administration, document exploration, index management, and visualization. While dashboards support charts and visualizations, they are also useful for searching documents, monitoring the cluster, managing indices, and validating that our vulnerability data has been indexed correctly.





**# How Query Building and Opensearch Search Engine Works ?**

* FastAPI is responsible for constructing the search query based on the user's request. It sends this query to OpenSearch using the OpenSearch client API. OpenSearch then executes the query against its indexed vulnerability records.
* Internally, OpenSearch uses ***Apache Lucene's inverted indexing*** mechanism, so it does not scan every document sequentially. Instead, it looks up the ***indexed terms***, retrieves the matching document IDs, and returns only the relevant vulnerability records.
* In our project, we did not implement the search algorithm ourselves—we leveraged OpenSearch's indexing and search capabilities while designing the data model, indexing workflow, and query logic that powers vulnerability search and SBOM correlation.





**# What is an Inverted Index? Can you explain it with a real example?**

* An Inverted Index is the core data structure used by OpenSearch and Apache Lucene for high-performance searching. Instead of storing data as Document → Words, it stores Word → Documents.
* During indexing, OpenSearch analyzes each vulnerability record and builds an index that maps searchable terms such as CVE IDs, products, vendors, and keywords to the documents in which they appear.
* When a user searches for a term like "log4j", OpenSearch looks up that term in the inverted index and immediately retrieves the matching document IDs instead of scanning every vulnerability record.
* In our project, we did not implement the indexing algorithm ourselves; we used OpenSearch's built-in indexing capabilities by storing normalized vulnerability records and querying them through FastAPI. This enables fast retrieval of relevant vulnerability information even when the database contains hundreds of thousands of CVE records.





**# Which commands you used in your project for opensearch ?**

| Command                      | Purpose                   | Used in Project      |

| ---------------------------- | ------------------------- | -------------------- |

| GET /                        | Check OpenSearch          |  yes                 |

| GET \_cat/indices?v           | List indices              |  yes                 |

| GET vulnerabilities/\_search  | Search CVEs               |  yes                 |

| POST vulnerabilities/\_doc    | Store VulnerabilityRecord |  (via Python client) |

| GET vulnerabilities/\_count   | Count indexed CVEs        |  yes                 |

| GET vulnerabilities/\_mapping | Verify schema             |  yes                 |

| GET \_cluster/health          | Health check              |  yes                 |

| DELETE vulnerabilities       | Reset testing database    | During development   |

| GET \_stats                   | Performance monitoring    | Optional             |

| POST \_analyze                | Tokenization testing      | Optional             |



**# FastAPI sends the request to OpenSearch. But what happens inside OpenSearch after that?**

When FastAPI receives a request, it constructs an OpenSearch Query DSL request and sends it to the OpenSearch server using the official OpenSearch Python client over HTTP. Inside OpenSearch, the request first passes through the query parser, which validates the query structure and determines the query type. The query terms are then processed by an analyzer, which performs operations such as tokenization and lowercasing. OpenSearch converts the Query DSL into an internal Lucene query and searches its inverted index rather than scanning every document. Matching documents are ranked using the BM25 relevance algorithm, the corresponding documents are retrieved from storage, and the results are returned as a JSON response. FastAPI then formats this response and sends it back to the frontend. This architecture enables very fast searches even when the vulnerability database contains hundreds of thousands of indexed CVE records.

