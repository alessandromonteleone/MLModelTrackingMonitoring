# MLModel Tracking and Monitoring

This repository supports tracking and optimizing machine learning experiments in a distributed production environment, 
with a primary focus on parameter optimization through a *Grid Search* approach. It enables emulation and monitoring of 
an ML model across various stages in production, simulating workflows with *training*, *testing*, and *production* data.

The system integrates with **MLflow** for:

- Logging model parameters and metrics via API,
- Accessing experiment data for analysis and comparison,
- Selecting the best-performing model and deploying it in a simulated production environment.

**Evidently AI** is also leveraged to test and monitor both data and the ML model in a production environment, evaluating and tracking the quality of data and predictions.

## Core Components

The system comprises four primary containerized microservices:

- **MySQL Database**: Stores and organizes all relevant experiment data;
- **Simulator**: Generates simulated *training*, *testing*, and *production* datasets;
- **ML Model Service**: Manages a trainable ML model that can be evaluated on simulated data batches;
- **Tracking Service**: Handles experiment optimization, logging, and retrieval of metadata and artifacts;
- **Monitoring Service**: To observe the model, allowing to compute reports and tests and trigger model re-training.

This setup enables comprehensive tracking and monitoring of machine learning models in a production-like environment, supporting both experimentation and long-term model maintenance.

## Index
- [MLModelTrackingMonitoring](#ML Model Tracking and Monitoring)
    - [Index](#index)
    - [Installation](#installation)
    - [Dependencies](#dependencies)
    - [Usage](#usage)
    - [References](#references)


## Installation
To clone the repository:
```
git clone https://github.com/alessandromonteleone/MLModelTrackingwithinMicroservicesEnvironments.git
```


## Dependencies
To create a virtual environment and install all required dependencies:

```
python -m venv venv && source venv/bin/activate && pip install -r requirements.txt
```
Each microservice also has its own `requirements.txt` file, along with a `config.yml` configuration file and a `Dockerfile` to build the corresponding Docker container.

## Usage
To start and stop all containers in background mode:

- `./launch.sh`

To test all the micro-services *API*, please download and install `Postman` to send requests.


## References
- [Docker](https://www.docker.com/)
- [Postman](https://www.postman.com/)