***# Docker Compose mainly has three sections:***

docker-compose.yml

├── Services (Containers)

├── Volumes  (Persistent Storage)

└── Networks (Communication)



**# SERVICES:**

This tells Docker: "These are the containers I want to run."

services

├── opensearch

├── dashboards

├── fastapi

└── scheduler



Each becomes an independent Docker container.

Think of them as four different Linux machines running inside your EC2.



***I) Opensearch Service:*** 

* OpenSearch is a ***Java application***.
* Search engine/database of UniVulner.
* Every search from FastAPI eventually reaches this container.
* ***image: opensearchproject/opensearch:2.19.1:*** 

&#x09;If the image already exists locally >> Docker uses it. 

&#x09;Otherwise >> Docker downloads it from Docker Hub.

* ***container\_name: opensearch***: set container name so easy for administration
* ***environment:*** Environment variables configure how OpenSearch behaves.

&#x09;***1. discovery.type=single-node:*** Tells OpenSearch: "Don't look for other nodes. Run independently." Without this, OpenSearch would wait for other cluster nodes and may not start correctly.

&#x09;***2. bootstrap.memory\_lock=true:*** requests the operating system to keep the OpenSearch memory in RAM instead of allowing it to be swapped or in Disk.

&#x09;***3. OPENSEARCH\_JAVA\_OPTS=-Xms2g -Xmx2g:*** Because OpenSearch runs on Java, it uses the Java Heap. So we set Initial heap = 2 GB, Maximum heap = 2 GB

&#x09;***4. DISABLE\_SECURITY\_PLUGIN=true:*** That means your OpenSearch instance doesn't require login credentials for easier development.

&#x09;***5. path.repo=/usr/share/opensearch/snapshots:*** This setting tells OpenSearch where snapshot repositories are located inside the container.



Host IP : Host Port : Container Port:

* ***127.0.0.1:9200:9200:*** Only processes running on the EC2 host itself can access this service.
* ***127.0.0.1:9600:9600:*** Port 9600 is used by the OpenSearch Performance Analyzer and monitoring APIs. It is also used/restricted to localhost.
* ***mem\_limit: 4g:*** Limits the maximum memory this container may consume.
* ***logging:*** Docker stores container logs in driver: json-file file upto 100MB in size.





**II) *Dashboards Service*:**

| ---------------------------------- | ------------------------------------------------------------------------------------------------------------- |

| Configuration                      | Purpose in UniVulner                                                                                          |

| ---------------------------------- | ------------------------------------------------------------------------------------------------------------- |

| image                              | Runs the OpenSearch Dashboards UI (version 2.19.1).                                                           |

| container\_name                     | Gives the container a fixed, easy-to-manage name.                                                             |

| OPENSEARCH\_HOSTS                   | Tells Dashboards which OpenSearch instance to connect to (http://opensearch:9200).                            |

| DISABLE\_SECURITY\_DASHBOARDS\_PLUGIN | Disables Dashboard authentication to match the disabled OpenSearch security plugin in your development setup. |

| ports                              | Exposes Dashboard on port 5601, but only to the local host (127.0.0.1).                                 	     |

| depends\_on                         | Starts Dashboard only after OpenSearch passes its health check.                                               |

| restart                            | Automatically restarts the service after crashes or system reboots.                                           |

| mem\_limit                          | Limits Dashboard memory usage to 1 GB.                                                                        |

| logging                            | Rotates log files to prevent disk space exhaustion.                                                           |

| networks                           | Connects Dashboard to the private Docker bridge network for communication with OpenSearch.                    |

| ---------------------------------- | ------------------------------------------------------------------------------------------------------------- |



***III) FastAPI Service:***

| ------------------- | -------------------------------------------------------------------------------------------------------------- |

| Configuration       | Purpose in UniVulner                                                                                           |

| ------------------- | -------------------------------------------------------------------------------------------------------------- |

| image               | Pulls the pre-built FastAPI application image from ***GitHub Container Registry (GHCR).***                           |

| pull\_policy: always | Always checks GHCR for the latest image before starting the container.                                         |

| container\_name      | Assigns a fixed name (fastapi) for easier management.                                                          |

| init: true          | Runs a lightweight init process for proper signal handling and zombie process cleanup.                         |

| env\_file            | Loads environment variables from the .env file without hardcoding them in the Compose file.                    |

| ports               | Exposes the FastAPI application on port 8000 so users can access the REST APIs.                                |

| depends\_on          | Waits until OpenSearch is healthy before starting FastAPI.                                                     |

| restart             | Automatically restarts the container after failures or system reboots.                                         |

| mem\_limit           | Restricts FastAPI to 1 GB of RAM.                                                                              |

| logging             | Rotates log files to prevent excessive disk usage.                                                             |

| networks            | Connects FastAPI to the private Docker bridge network, allowing communication with OpenSearch by service name. |

| ------------------- | -------------------------------------------------------------------------------------------------------------- |



***IV) Scheduler Service:***

| ------------------- | ----------------------------------------------------------------------------------------------------------- |

| Configuration       | Purpose in UniVulner                                                                                        |

| ------------------- | ----------------------------------------------------------------------------------------------------------- |

| image               | Pulls the Scheduler application image from ***GitHub Container Registry (GHCR).***                                |

| pull\_policy: always | Ensures the latest Scheduler image is downloaded before startup.                                            |

| container\_name      | Assigns a fixed name (scheduler) for easier management.                                                     |

| env\_file            | Loads API keys and configuration from the .env file.                                                        |

| depends\_on          | Starts the Scheduler only after OpenSearch is healthy.                                                      |

| restart             | Automatically restarts the Scheduler after failures or system reboots.                                      |

| networks            | Connects the Scheduler to the private Docker bridge network so it can communicate with OpenSearch.          |

| No ports            | The Scheduler is an internal background service, so it doesn't need to accept incoming network connections. |

| ------------------- | ----------------------------------------------------------------------------------------------------------- |





***# Why bind Dashboard to 127.0.0.1?***

To prevent direct public access. Only processes running on the EC2 host can access the Dashboard interface, reducing the attack surface.



***# Why use GHCR instead of building on the EC2 server?***

Building the image once in GitHub and storing it in GitHub Container Registry ensures consistent deployments across environments. The server only downloads the pre-built image, making deployments faster and reducing build dependencies on the production server.



***# Why doesn't Scheduler expose a port?***

The Scheduler is not a user-facing service. It performs background tasks and communicates only with external threat intelligence sources and OpenSearch. Since no client needs to connect directly to it, exposing a port is unnecessary and would increase the attack surface.



***# Why does Scheduler need OpenSearch to be healthy first?***

The Scheduler's primary responsibility is to write enriched vulnerability data into OpenSearch. If OpenSearch is unavailable, synchronization cannot complete successfully. The health check ensures that OpenSearch is ready before synchronization begins.

