Our application consists of four containers connected through a private Docker bridge network.



1. **OpenSearch:** It's out database, stores actual data by scheduler and provide data to FASTAPI. (Port: 9200)
2. **OpenSearch Dashboards/Kibana:** It is a web-based visualization tool that connects to the OpenSearch server. (Port: 5601)
3. **FastAPI:** exposes REST APIs for vulnerability search and software inventory assessment. (Port: 8000)
4. **Scheduler:** periodically synchronizes threat intelligence from external sources.



All containers communicate through the internal **Docker bridge network** called ***vulnerability-network***, allowing services to communicate using container names instead of IP addresses and internal traffic remains isolated from external networks unless specific ports are published.





***# Docker Compose Architecture:***

&#x20;               Threat Sources

&#x20;                      │

&#x20;               Scheduler

&#x20;                      │

&#x20;               writes documents

&#x20;                      │

&#x20;                      ▼

&#x20;                OpenSearch (Port: 9200)

&#x20;                ▲         │

&#x20;        queries │         │ visualizes

&#x20;                │         ▼

&#x20; (Port: 8000) FastAPI   Dashboards/Kibana (Port: 5601)

&#x20;                ▲

&#x20;                │

&#x20;           REST API

&#x20;                ▲

&#x20;                │

&#x20;             Browser





***# Why separate FastAPI and Scheduler into different containers?***

The FastAPI service handles user requests, while the Scheduler performs periodic background synchronization with threat intelligence sources. Separating them prevents long-running synchronization tasks from affecting API responsiveness and allows each service to be managed or restarted independently.





***# Why use Docker Compose?***

Docker Compose simplifies deployment by defining all services, networks, volumes, environment variables, dependencies, and restart policies in a single configuration file. This allows the complete platform to be started consistently using a single command.

**Commands Used:**

docker compose up/down/stop -d

docker compose up/down/stop scheduler



***# What is mean: Your compose file contains:***

depends\_on:

&#x20; opensearch:

&#x20;     condition: service\_healthy

Answer: FastAPI and the Scheduler depend on OpenSearch. The health check ensures that OpenSearch is fully initialized before dependent services start, preventing connection failures during application startup.



***# Why did you create a volume?***

Docker containers are ephemeral/Temporary stores data. If an OpenSearch container is removed, its internal filesystem is lost. Using a persistent Docker volume ensures that indexed vulnerability data remains available even if the container is recreated or updated.



**# What is restart: unless-stopped in Your compose file?**

This policy automatically restarts the service after unexpected failures or system reboots, improving availability without requiring manual intervention.



***# Why bind OpenSearch to localhost instead of exposing it publicly? (127.0.0.1:9200:9200)***

OpenSearch stores sensitive vulnerability intelligence and should not be directly accessible from external networks. Binding it to localhost limits access to the host itself, reducing the attack surface. User requests are intended to go through the application layer rather than directly to OpenSearch.



**command to access them**: ssh -i "LocalPath\\univulner.pem" -L 5601:localhost:5601 -L 9200:localhost:9200 ubuntu@BACKEND\_IP

