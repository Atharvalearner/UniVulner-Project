***SBOM/Inventory Assessment module:***



Users upload a CSV file containing installed software from different systems.



The platform compares the software vendor, product, and version against the vulnerability database.



Initially, matching was based only on vendor and product, but later I enhanced it by extracting affected version ranges from NVD CPE data and implementing version-aware comparison.



This significantly reduced false positives by reporting vulnerabilities only when the installed version falls within the affected version range.

