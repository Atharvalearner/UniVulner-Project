***# Theory:***

UniVulner follows a layered deployment architecture where each component has a specific responsibility.



When a user accesses the platform through a browser, the request first reaches the web server hosted in AWS.



The request is then forwarded to the frontend application. When the user searches for a vulnerability, the frontend sends an API request to the backend service.



The backend processes the request, queries OpenSearch for the required vulnerability intelligence, and returns the enriched results to the frontend.



Finally, the frontend presents the consolidated vulnerability information to the analyst.





&#x20;              Security Analyst

&#x20;                      │

&#x20;              HTTPS Request

&#x20;                      ▼

&#x20;              Internet / DNS

&#x20;                      ▼

&#x20;              AWS Load Balancer

&#x20;                      ▼

&#x20;            Reverse Proxy (Nginx)

&#x20;         ┌─────────┴─────────┐

Frontend Container         Backend API Container

&#x20;                                  ▼

&#x20;                          OpenSearch Cluster

&#x20;                                  ▼

&#x20;                     	   Vulnerability Intelligence





***# Can users directly access OpenSearch from their browser?***

No. The browser communicates only with the frontend and backend. OpenSearch remains in the private network and accepts requests only from the backend service. This prevents unauthorized access and enforces security controls.



***A strong way to conclude is:***

I designed the deployment following the principle of defense in depth. Public-facing components are limited to the Load Balancer and web server, while backend services and OpenSearch remain protected inside the private network. This reduces the attack surface and aligns with enterprise security best practices.

