<a href="https://github.com/newrelic/open-source-office/blob/master/examples/categories/index.md#category-new-relic-experimental">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/newrelic/open-source-office/master/examples/categories/images/dark/Experimental.png">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/newrelic/open-source-office/master/examples/categories/images/Experimental.png">
  <img alt="New Relic open source - Experimental" src="https://raw.githubusercontent.com/newrelic/open-source-office/master/examples/categories/images/Experimental.png">
</picture>
</a>

# ML Performance Monitoring
ml-performance-monitoring provides a Python library for sending machine learning models' inference data and performance metrics into New Relic. It is based on the [newrelic-telemetry-sdk-python](https://github.com/newrelic/newrelic-telemetry-sdk-python) library. By using this package, you can easily and quickly monitor your model, directly from a Jupyter notebook or a cloud service. 

## Getting Started
- [Documentation](https://docs.newrelic.com/docs/mlops/bring-your-own/mlops-byo/) - Overview of the New Relic MLOps docs and related resources
<!--- - TODO - add demo video [Demo: Intro to New Relic MLOps Demo Video](https://...) - Learn by doing! In under 15 minutes, you'll see how you can get your models in observability--->
- [Example Notebooks](https://github.com/newrelic-experimental/ml-performance-monitoring/blob/main/examples/XGBoost_on_Boston_housing_prices_dataset.ipynb) - Try out our XGBoost model on [Boston housing prices](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_boston.html) dataset. [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/newrelic-experimental/ml-performance-monitoring/blob/main/examples/XGBoost_on_Boston_housing_prices_dataset.ipynb)
- [Example notebook](https://github.com/newrelic-experimental/ml-performance-monitoring/blob/main/examples/sklearn.RandomForestClassifier_on_Iris_dataset.ipynb) - Take a look at how 24 hours of model inference data simulation looks using New Relic MLOps.  [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/newrelic-experimental/ml-performance-monitoring/blob/main/examples/sklearn.RandomForestClassifier_on_Iris_dataset.ipynb)
- [Additional Guides](https://github.com/newrelic/newrelic-telemetry-sdk-python) - Learn about New Relic's Telemetry Software Development Kit

<!---
## GIF
TODO - add a gif example of our machine learning model dashboard in NR
​--->

## Installation
​
**With `pip`**

```bash
pip install git+https://github.com/newrelic-experimental/ml-performance-monitoring.git
```
<!---**With `conda`**

```sh
TODO - add conda installation code
```--->

## Quickstart

 [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/newrelic-experimental/ml-performance-monitoring/blob/main/examples/XGBoost_on_Boston_housing_prices_dataset.ipynb) notebook, which showcases an end-to-end example of usage for  models.


```python

# STEP 1: Initialize the monitoring.
ml_monitor = MLPerformanceMonitoring(...)

# STEP 2: Add your algorithm.
y = my_model.predict(X)

# STEP 3: Record your data
ml_monitor.record_inference_data(X, y)
```
​
## Usage

#### STEP 1: Pip Install
```bash
pip install git+https://github.com/newrelic-experimental/ml-performance-monitoring.git
```

#### STEP 2: Set Your Environment Variable 
Set your environment variable as: NEW_RELIC_INSERT_KEY
(this can be license-key or insights-insert-key.)
[Click here to learn how to get your insert key](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys/#insights-insert-key).

Are you using an EU New Relic accout? click [here](https://github.com/iazam6/ml-performance-monitoring/edit/readme-update-v1/README.md#eu) for more instructions.

#### STEP 3: Import package
```python
from ml_performance_monitoring.monitor import MLPerformanceMonitoring
```

#### STEP 4: Create model monitor
```python
metadata = {"version": "1.0"}
features_columns, labels_columns = (
    ["feture_1", "feture_2", "feture_3", "feture_4"],
    ["target"],
)

ml_monitor_ = MLPerformanceMonitoring(
    insert_key=None,  # set the environment variable NEW_RELIC_INSERT_KEY or send your insert key here
    model_name="my stunning model",
    metadata=metadata,
    features_columns=features_columns,
    labels_columns=labels_columns,
    label_type="numeric",
)
```

#### STEP 5: Do your thing.
```python
y = my_model.predict(X)
```

#### STEP 6: Record
```python
ml_performence_monitor_model.record_inference_data(X, y)
```

#### STEP 7: Monitor
Done! Check your application in the [New Relic UI](https://one.newrelic.com/nr1-core?filters=%28domain%20%3D%20%27MLOPS%27%20AND%20type%20%3D%20%27MACHINE_LEARNING_MODEL%27%29) to see the real time data.




### EU Account Users
If you are using an EU account, send it as a parameter at the MLPerformanceMonitoring call if your environment variable is not set:
* ``EVENT_CLIENT_HOST`` and ``METRIC_CLIENT_HOST``
  * US region account (default)-
    * ``EVENT_CLIENT_HOST``: insights-collector.newrelic.com
    * ``METRIC_CLIENT_HOST``: metric-api.newrelic.com
  * EU region account-
    * ``EVENT_CLIENT_HOST``: insights-collector.eu01.nr-data.net
    * ``METRIC_CLIENT_HOST``: metric-api.eu.newrelic.com/metric/v1
    
It can also be sent as parameters at the MLPerformanceMonitoring call.
​
## FAQ
### Support
As an open source library, customers can interact with New Relic employees as well as other customers to get help by opening GitHub issues in the repository.
​
### Contributing
We encourage your contributions to improve ml-performance-monitoring! Keep in mind when you submit your pull request, you'll need to sign the CLA via the click-through using CLA-Assistant. You only have to sign the CLA one time per project. If you have any questions, or to execute our corporate CLA (required if your contribution is on behalf of a company) please drop us an email at opensource@newrelic.com.

**A note about vulnerabilities:**
As noted in our [security policy](https://github.com/newrelic-experimental/ml-performance-monitoring/security/policy), New Relic is committed to the privacy and security of our customers and their data. We believe that providing coordinated disclosure by security researchers and engaging with the security community are important means to achieve our security goals.

If you believe you have found a security vulnerability in this project or any of New Relic's products or websites, we welcome and greatly appreciate you reporting it to New Relic through [HackerOne](https://hackerone.com/newrelic).
​
## License
ml-performance-monitoring is licensed under the [Apache 2.0](http://apache.org/licenses/LICENSE-2.0.txt) License.

**If applicable:** 
The ml-performance-monitoring also uses source code from third-party libraries. You can find full details on which libraries are used and the terms under which they are licensed in the third-party notices document.
​
