## Emulating the Lifecycle

The goal of this project is to emulate the entire lifecycle of an *ML model* in a *production environment*, from tracking
**model optimizations** to selecting the best-performing model, comparing **stored results** and perform **monitoring**. By logging and making these records
easily accessible, it enables streamlined model selection and performance analysis. 
Using [`Postman`](https://www.postman.com/), different requests can be sent to each *microservice* to manage and interact with each stage of this lifecycle.




### Data Loading

```
POST request on http://127.0.0.1:5000/root/simulator
Request body: {
  "set_name": "training"
}
```

```
POST request on http://127.0.0.1:5000/root/simulator
Request body: {
  "set_name": "testing"
}
```
### MLFlow Logging

 **Training set must be loaded** on the *MySQL DB dataset*, in order to train *ML model* and perform the
hyperparameter tuning.

#### Hyperparameter Tuning and Logging Expreiment

We then send a **POST** request to initiate the model optimization specifying:
- the models
- hyperparameters
- grid search instructions (cross-validation and scoring metric)
- and logging details for experiment tracking in MLflow.

```
POST request on http://127.0.0.1:5002/root/tracking/model_management
Request body: {
  "request_information": "optimization",
  "experiment_name": "wine_test_1",
  "experiment_description": "Grid Search Optimization - Cross Validation Performance",
  "experiment_owner": "Ale",

  "models": ["LogisticRegression", "RandomForest", "SVC"],
  "hyperparameters": {
    "LogisticRegression": {
      "C": [0.1, 1],
      "solver": ["liblinear"]
    },
    "RandomForest": {
      "n_estimators": [ 1, 3],
      "max_depth": [null, 3],
      "min_samples_split": [2, 5]
    },
    "SVC": {
      "C": [0.1, 10],
      "kernel": ["linear", "rbf"],
      "gamma": ["scale", "auto"]
    }
  },

  "scoring": "accuracy",
  "cv": 5
}
```
Response:
```
{
    "message": "Experiments retrieved successfully",
    "data": {
        "experiment_id": "1",
        "run_ids": {
            "LogisticRegression": "5f69f7ee0507453e9787991521adeb82",
            "RandomForest": "2d8371356f1f4665b7697d732f5da3f3",
            "SVC": "0b9f23e1f0c046d39b39e4b0add4ac95"
        }
    }
}
```

It is essential to save both the *experiment ID* and the *runs IDs*. However, in any case, they can still be retrieved through simple GET requests.

### MLFlow Data Recovery

#### Recover all saved experiments with limited information:

```
GET request on http://127.0.0.1:5002/root/tracking/experiments
```
Response:
```
{
    "message": "Experiments retrieved successfully",
    "data": [
        {
            "experiment_id": "1",
            "name": "wine_test",
            "lifecycle_stage": "active",
            "run_count": 3
        }
    ]
}
```
#### Recovery all Runs of a specify Experiments througth Id
```
GET request on http://127.0.0.1:5002/root/tracking/runs?experiment_id=1
```
Response:
```
{
    "message": "Experiment Runs Retrieved",
    "data": [
        {
            "run_id": "0b9f23e1f0c046d39b39e4b0add4ac95",
            "status": "FINISHED",
            "start_time": "2024-10-30 20:44:21",
            "end_time": "2024-10-30 20:44:22",
            "run_name": "SVC Grid_Search_Opt and CV_Performance",
            "tags": {
                "model_type": "SVC",
                "experiment_type": "GridSearch_CrossValidation"
            }
        },
        {
            "run_id": "2d8371356f1f4665b7697d732f5da3f3",
            "status": "FINISHED",
            "start_time": "2024-10-30 20:44:19",
            "end_time": "2024-10-30 20:44:21",
            "run_name": "RandomForest Grid_Search_Opt and CV_Performance",
            "tags": {
                "model_type": "RandomForest",
                "experiment_type": "GridSearch_CrossValidation"
            }
        },
        {
            "run_id": "5f69f7ee0507453e9787991521adeb82",
            "status": "FINISHED",
            "start_time": "2024-10-30 20:44:16",
            "end_time": "2024-10-30 20:44:19",
            "run_name": "LogisticRegression Grid_Search_Opt and CV_Performance",
            "tags": {
                "model_type": "LogisticRegression",
                "experiment_type": "GridSearch_CrossValidation"
            }
        }
    ]
}
```

#### Recover information about a specified *Experiment*:

- The `request_information` can be:

1. `general`: for obtaining general information about experiments.
2. `best_model`: to get the runs related to the model that achieved the best performance during cross-validation. In this case, specifying the `filter` for the metric to consider is required.
3. `statistics`: to obtain various statistics about metrics such as mean, median, standard deviation, variance, min, max, mode, and IQR. You can use `filter` to specify which metrics to consider; to get a complete report of statistics, set `filter` to 'all' or omit it.

```
POST request on http://127.0.0.1:5002/root/tracking/experiments
{
    "experiment_id": "1",
    "request_information": "general"
}
```

response:
```
{
    "message": "Information Retrieved",
    "data": {
        "experiment_info": {
            "experiment_id": "1",
            "experiment_name": "wine_test",
            "artifact_location": "/app/mlruns/1",
            "lifecycle_stage": "active",
            "tags": {
                "description": "Grid Search Optimization - Cross Validation Performance",
                "owner": "Ale"
            },
            "data_creation": "2024-10-30 20:44:16",
            "last_updated": "2024-10-30 20:44:16"
        },
        "experiment_runs": {
            "number_of_runs": 3,
            "completed_runs": 3,
            "failed_runs": 0,
            "active_runs": 0,
            "stopped_runs": 0,
            "first_run_completed": "2024-10-30 20:44:16",
            "last_run_completed": "2024-10-30 20:44:22"
        }
    }
}
```
---
```
POST request on http://127.0.0.1:5002/root/tracking/experiments
{
    "experiment_id": "1",
    "request_information": "best_model",
    "filter": ["model_accuracy"]
}
```
response:
```
{
    "message": "Best model and run retrieved successfully.",
    "data": {
        "run_id": "5f69f7ee0507453e9787991521adeb82",
        "run_name": "LogisticRegression Grid_Search_Opt and CV_Performance",
        "model": "LogisticRegression",
        "metrics": {
            "model_accuracy": 0.9245283018867925
        }
    }
}
```
---
```
POST request on http://127.0.0.1:5002/root/tracking/experiments
{
    "experiment_id": "1",
    "request_information": "statistics",
    "filter": ["all"]
}
```
response:
```
{
    "message": "Statistics Retrieved",
    "data": {
        "duration": {
            "mean": 0.3225320180257161,
            "median": 0.22951197624206543,
            "std_dev": 0.24966742678439094,
            "variance": 0.06233382399713921,
            "min": 0.0740654468536377,
            "max": 0.6640186309814453,
            "mode": 0.6640186309814453,
            "iqr": 0.5899531841278076
        },
        "accuracy": {
            "mean": 0.9088050314465409,
            "median": 0.9150943396226415,
            "std_dev": 0.016034652558467873,
            "variance": 0.0002571100826707803,
            "min": 0.8867924528301887,
            "max": 0.9245283018867925,
            "mode": 0.9150943396226415,
            "iqr": 0.037735849056603765
        },
        "f1_score": {
            "mean": 0.9079266853127027,
            "median": 0.9145840674968364,
            "std_dev": 0.016324599192037065,
            "variance": 0.0002664925387806572,
            "min": 0.8854538500764916,
            "max": 0.9237421383647799,
            "mode": 0.9145840674968364,
            "iqr": 0.03828828828828834
        },
        "precision": {
            "mean": 0.9108102003255284,
            "median": 0.9189476849854208,
            "std_dev": 0.016590439513733773,
            "variance": 0.0002752426832588589,
            "min": 0.8876836503450208,
            "max": 0.9257992656461439,
            "mode": 0.9189476849854208,
            "iqr": 0.03811561530112306
        },
        "recall": {
            "mean": 0.9088050314465409,
            "median": 0.9150943396226415,
            "std_dev": 0.016034652558467873,
            "variance": 0.0002571100826707803,
            "min": 0.8867924528301887,
            "max": 0.9245283018867925,
            "mode": 0.9150943396226415,
            "iqr": 0.037735849056603765
        }
    }
}
```
#### Recover Information about a Specified *Run*

To retrieve information about a specific *Run*, you need to provide the `run_id` in addition to the `experiment_id`. The `request_information` can be specified as follows:

1. **`parameters`**: This retrieves the parameters defined by the `type` field, which can be:
   - **`data`**: Use this to obtain parameters such as the number of data samples, features, and the different classes of data utilized during hyperparameter tuning.
   - **`grid_search`**: Select this to get the scoring, cross-validation details, and all model parameters used in the grid search.
   - **`model`**: Use this option to retrieve the parameters that were optimized during the model training process.

```
POST request on http://127.0.0.1:5002/root/tracking/runs
{
    "experiment_id": "1",
    "run_id": "5f69f7ee0507453e9787991521adeb82",
    "request_information": "parameters",
    "type": "data"
}
```
response:
```
{
    "message": "Parameters Retrieved",
    "data": {
        "classes": "3",
        "features": "13",
        "samples": "106"
    }
}
```
---
```
POST request on http://127.0.0.1:5002/root/tracking/runs
{
    "experiment_id": "1",
    "run_id": "5f69f7ee0507453e9787991521adeb82",
    "request_information": "parameters",
    "type": "grid_search"
}
```
response:
```
{
    "message": "Parameters Retrieved",
    "data": {
        "C": "[0.1, 1]",
        "cv_folds": "5",
        "scoring": "accuracy",
        "solver": "['liblinear']"
    }
}
```
---
```
POST request on http://127.0.0.1:5002/root/tracking/runs
{
    "experiment_id": "1",
    "run_id": "5f69f7ee0507453e9787991521adeb82",
    "request_information": "parameters",
    "type": "model"
}
```
response:
```
{
    "message": "Parameters Retrieved",
    "data": {
        "C": "0.1",
        "solver": "liblinear"
    }
}
```
---

2. **`metrics`**: This retrieves metrics defined by the `type` field, which can include:
   - **`system`**: This option retrieves CPU usage data during the processing.
   - **`model`**: This retrieves all metrics computed during cross-validation.

```
POST request on http://127.0.0.1:5002/root/tracking/runs
{
    "experiment_id": "1",
    "run_id": "5f69f7ee0507453e9787991521adeb82",
    "request_information": "metrics",
    "type": "system"
}
```
response:
```
{
    "message": "Parameters Retrieved",
    "data": {
        "cpu_usage": 18.6,
        "execution_time": 0.0740654468536377
    }
}
```
---
```
POST request on http://127.0.0.1:5002/root/tracking/runs

{
    "experiment_id": "1",
    "run_id": "5f69f7ee0507453e9787991521adeb82",
    "request_information": "metrics",
    "type": "model"
}
```
response:
```
{
    "message": "Parameters Retrieved",
    "data": {
        "accuracy": 0.9245283018867925,
        "f1_score": 0.9237421383647799,
        "precision": 0.9257992656461439,
        "recall": 0.9245283018867925
    }
}
```
### Setting and Use of ML Model


#### Set Model for **ML** micro-service
Through the **Tracking** microservice, a model can be set up in the **ML** microservice
```
POST request on http://127.0.0.1:5002/root/tracking/model_management
{
    "request_information":"setting",
    "run_id":"5f69f7ee0507453e9787991521adeb82"
}
```

#### Using Model in ML service

Now, the *ML model* can be trained and tested.

```
POST request on http://127.0.0.1:5001/root/ml
Request body: {
  "dataset":"training"
}
```

#### Testing

Now, the *ML model* can be tested on the *testing* set.

```
POST request on http://127.0.0.1:5001/root/ml
Request body: {
  "dataset": "testing"
}
```

### ML Model in a Production Environment

Having a well performing *ML model*, it is feasible to put it in a *production environment*.
It is now possible also load the *production* set, that is the final *batch* of dirty data. This data simulate the new and unseen data that come to the *ML model* once it is put in a *production environment*.
```
POST request on http://127.0.0.1:5000/root/simulator
Request body: {
  "set_name": "production"
}
```

The *ML model* can be tested on the *production* set. Very often *production* data, over time, tend to differ from the data on which the *ML model* was trained, thus causing a decline in performance.
```
POST request on http://127.0.0.1:5001/root/ml
Request body: {
  "dataset": "production"
}
```

### ML Model Monitoring

Finally, through *Evidently AI* it is possible to compute *reports* and *tests*, to detect the data *drift* and many others problems that could be present on *production* data. Specifically, *summary* can be used to check if a *re-training* is necessary. It is therefore allowed to choose the specific *tests* to perform.
```
POST request on http://127.0.0.1:5002/ml_model_monitoring/monitoring
Request body: {
  "metric": "report"
}
```
```
POST request on http://127.0.0.1:5003/root/monitoring
Request body: {
  "metric": "tests"
}
```
```
POST request on http://127.0.0.1:5003/root/monitoring
Request body: {
  "metric": "summary",
  "tests": {
    "data_drift": true,
    "data_quality": true,
    "data_stability": true,
    "no_target_performance": true,
    "multi_class_classification": true
  }
}
```

### Deleting MLFlow Experiments and Runs

Additionally, if desired, **Experiments** or **Runs** can also be deleted from the MLflow database using the following
**DELETE** command.

```
DELETE request on http://127.0.0.1:5002/root/tracking/experiments?experiment_id=1
```
```
DELETE request on http://127.0.0.1:5002/root/tracking/runs?experiment_id=1&run_id=5f69f7ee0507453e9787991521adeb82
```