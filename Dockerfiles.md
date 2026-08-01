***1. FastAPI Dockerfile:***

| -------------------------------------------------- | ------------------------------------------------------------------------------------------------- |

| Instruction                                        | Purpose in UniVulner                                                                              |

| -------------------------------------------------- | ------------------------------------------------------------------------------------------------- |

| FROM python:3.11.10-slim                           | Uses a lightweight Python 3.11 base image to reduce image size and improve deployment efficiency. |

| WORKDIR /app                                       | Sets /app as the working directory inside the container.                                          |

| COPY requirements.txt .                            | Copies only the dependency file first to take advantage of Docker layer caching.                  |

| RUN pip install --no-cache-dir -r requirements.txt | Installs all required Python packages while avoiding unnecessary pip cache files.                 |

| COPY . .                                           | Copies the complete UniVulner source code into the container.                                     |

| EXPOSE 8000                                        | Documents that the FastAPI application listens on port \*\*8000\*\*.                                  |

| CMD \["uvicorn", "api.main:app", "--host",          | Starts the Uvicorn server, which runs the FastAPI application and accepts incoming API requests.  |

|      "0.0.0.0", "--port", "8000"] 	             | 												         |

| -------------------------------------------------- | ------------------------------------------------------------------------------------------------- |



***2. Initial Loader Dockerfile:***

It is same as FastAPI Dockerfile just last line is change:

CMD \["python", "-m", "run.initial\_load"]

Instead of executing python run/initial\_load.py Python executes the module run.initial\_load



***3. Scheduler Dockerfile:***

It is same as FastAPI Dockerfile just last line is change:

CMD \["python", "-m", "run.scheduler"]

Instead of executing python run/scheduler.py Python executes the module run.scheduler





***# Why did you choose python:3.11.10-slim?***

I chose the slim image because it contains the required Python runtime while keeping the image size smaller, reducing download time, storage usage, and potential security exposure.



***# Why do we need Initial Load?***

Initial Load populates an empty OpenSearch index with historical vulnerability data. Without it, the platform would have no existing vulnerability records, and users would not be able to search previously published CVEs.



***# Why doesn't Scheduler expose any ports?***

Because Nobody connects to Scheduler. It is not a web server. It performs background jobs only. Exposing a port would unnecessarily increase the attack surface.

