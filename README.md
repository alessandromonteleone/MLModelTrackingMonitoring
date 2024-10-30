# MLModelTracking

This repository enables tracking machine learning experiments in a distributed production environment, aiming to optimize model parameters through a *Grid Search* approach. The system leverages MLFlow for:

- Logging model parameters and metrics via API,
- Retrieving experiment data for comparison and analysis,
- Selecting the optimal model and deploying it in a simulated production environment using *training*, *testing*, and *production* data.

The system consists of 4 core containerized microservices:

- **MySQL Database**: to store and query all necessary data;
- **Simulator**: to generate *training*, *testing*, and *production* data;
- **ML Model**: a trainable model that can be tested on simulated data batches;
- **Tracking Service**: to manage optimization, logging, and retrieval of all metadata and artifacts related to the experiments.



## Index
- [MLModelMonitoring](#MLModelMonitoring)
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