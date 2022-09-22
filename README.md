Hello
[![New Relic Experimental header](https://github.com/newrelic/opensource-website/raw/master/src/images/categories/Experimental.png)](https://opensource.newrelic.com/oss-category/#new-relic-experimental)
# ml-performance-monitoring

>   ml-performance-monitoring provides a Python library for sending machine learning models' inference data and performance metrics into New Relic. It is based on the [newrelic-telemetry-sdk-python](https://github.com/newrelic/newrelic-telemetry-sdk-python) library. By using this package, you can easily and quickly monitor your model, directly from a Jupyter notebook or a cloud service.

## Installing ml_performance_monitoring
To start, the ml-performance-monitoring package must be installed. To install through pip:
```bash
    $ pip install git+https://github.com/newrelic-experimental/ml-performance-monitoring.git
```

## Example
1. The example code assumes you've set the following environment variables:

* ``NEW_RELIC_INSERT_KEY``- can be license-key or insights-insert-key ([how to get your insert key](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys/#insights-insert-key))

If not, please send it as a parameter at the MLPerformanceMonitoring call.

* ``EVENT_CLIENT_HOST`` and ``METRIC_CLIENT_HOST``
  * US region account (default)-
    * ``EVENT_CLIENT_HOST``: insights-collector.newrelic.com
    * ``METRIC_CLIENT_HOST``: metric-api.newrelic.com
  * EU region account-
    * ``EVENT_CLIENT_HOST``: insights-collector.eu01.nr-data.net
    * ``METRIC_CLIENT_HOST``: metric-api.eu.newrelic.com/metric/v1

can also be sent as parameters at the MLPerformanceMonitoring call.

2. The example uses the libraries: numpy, pandas, sklearn, xgboost


<br>

#### Imports, loading the dataset and training
```
from ml_performance_monitoring.monitor import wrap_model
import numpy as np
from sklearn.metrics import mean_squared_error
from sklearn.datasets import load_boston
from sklearn.model_selection import train_test_split
import xgboost as xgb

boston_dataset = load_boston()

X, y = (
    boston_dataset["data"],
    boston_dataset["target"],)

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=123
)

print(X_train[:5], y_train[:5])

xg_reg = xgb.XGBRegressor(
    objective="reg:linear",
    colsample_bytree=0.3,
    learning_rate=0.1,
    max_depth=5,
    alpha=10,
    n_estimators=10,
)

xg_reg.fit(X_train, y_train)
```
<br>


#### Sending inference data and metrics
```
metadata = {"environment": "aws", "dataset": "Boston housing prices", "version": "1.0"}
features_columns, labels_columns = (
    list(boston_dataset["feature_names"]),
    ["target"],
)

# Use the wrap_model() function to send your model or pipeline as parameters and use them as usual (fit, predict, etc.).
# This function will send your inference data and data_metrics automatically.


ml_performence_monitor_model = wrap_model(
    insert_key=None,  # set the environment variable NEW_RELIC_INSERT_KEY or send your insert key here
    model=xg_reg,
    model_name="XGBoost Regression on Boston Dataset",
    metadata=metadata,
    send_data_metrics=True,
    features_columns=features_columns,
    labels_columns=labels_columns,
    label_type="numeric",
)

y_pred = ml_performence_monitor_model.predict(
    X=X_test,
)

rmse = round(np.sqrt(mean_squared_error(y_test, y_pred)), 3)
print(f"RMSE: {rmse}")
metrics = {"RMSE": rmse,}

# Send your model metrics as a dictionary to new relic.
ml_performence_monitor_model.record_metrics(metrics=metrics)
```

[Check out the new entity that was created](https://one.newrelic.com/).
Alternatively, you can query your data in the [query builder](https://docs.newrelic.com/docs/query-your-data/explore-query-data/query-builder/use-advanced-nrql-mode-query-data/)
 using the following query:
```
Select * From InferenceData Where model_name='XGBoost Regression on Boston Dataset' Since 1 hour Ago Limit Max
```

For 24 hours of model inference data simulation, please run the [example notebook](https://github.com/newrelic-experimental/ml-performance-monitoring/blob/main/examples/sklearn.RandomForestClassifier_on_Iris_dataset.ipynb).

## Testing
```bash
pip install pytest
pytest tests
```

## Support
As an open source library, customers can interact with New Relic employees as well as other customers to get help by opening GitHub issues in the repository.

## Contributing
We encourage your contributions to improve ml-performance-monitoring! Keep in mind when you submit your pull request, you'll need to sign the CLA via the click-through using CLA-Assistant. You only have to sign the CLA one time per project.
If you have any questions, or to execute our corporate CLA (required if your contribution is on behalf of a company) please drop us an email at opensource@newrelic.com.

**A note about vulnerabilities:**

As noted in our [security policy](../../security/policy), New Relic is committed to the privacy and security of our customers and their data. We believe that providing coordinated disclosure by security researchers and engaging with the security community are important means to achieve our security goals.

If you believe you have found a security vulnerability in this project or any of New Relic's products or websites, we welcome and greatly appreciate you reporting it to New Relic through [HackerOne](https://hackerone.com/newrelic).

## License
ml-performance-monitoring is licensed under the [Apache 2.0](http://apache.org/licenses/LICENSE-2.0.txt) License.
>[If applicable: The ml-performance-monitoring also uses source code from third-party libraries. You can find full details on which libraries are used and the terms under which they are licensed in the third-party notices document.]
