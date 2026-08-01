The scheduler periodically triggers the refresh\_all\_sources() function. It retrieves the last synchronization timestamp for each source, performs an incremental fetch to get only new or modified CVEs, removes duplicate CVE IDs, refreshes those records through the processing pipeline, and finally updates the last synchronization timestamp. This keeps the vulnerability database up to date efficiently without reprocessing the entire dataset.





***# How does your system know whether a CVE has changed?***

The system stores the last successful synchronization time for each source. During the next refresh, it passes this timestamp to the corresponding collector, which uses the source's update mechanism to fetch only CVEs added or modified after that time. Any returned CVEs are then refreshed in OpenSearch with the latest information.



***# Where is the last synchronization time stored?***

The ***SyncManager*** is responsible for maintaining the last successful synchronization timestamp for each source. Before every refresh, it retrieves the stored timestamp, and after a successful synchronization, it updates it with the current time. This allows every collector to request only the changes since the previous synchronization.

