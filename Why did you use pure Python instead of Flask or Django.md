* Our project was divided into a data processing layer and an API layer. We implemented the data ingestion pipeline, normalization, enrichment, scheduler, GitHub correlation engine, and OpenSearch integration as plain Python modules ***because they perform background processing*** and don't require web framework features. 



* For the API layer, we **used FastAPI** instead of Flask or Django ***because it offers high performance, asynchronous request handling, automatic request validation with Pydantic, and built-in OpenAPI documentation***. Django would have introduced unnecessary components such as an ORM and template engine, while FastAPI provided exactly what our API-centric architecture required.





| Feature                | FastAPI     | Flask               | Django                |

| ---------------------- | ----------- | ------------------- | --------------------- |

| REST APIs              |  Excellent  |  Good               |  Good                 |

| Async Support          |  Native     | Limited             | Supported but heavier |

| Automatic Swagger Docs |  Built-in   | Requires extensions | Requires DRF          |

| Request Validation     |  Pydantic   | Manual              | Forms/Serializers     |

| ORM                    | Optional    | No                  | Built-in              |

| Best Fit for UniVulner |  Yes        | Possible            | Overkill              |



