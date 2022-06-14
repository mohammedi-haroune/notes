## Data scientists Pain Points

- Tracking: parameters, metrics and models
- Reproducibilty
- Tracebility  

## The concept of DevOps in ML

## GCP AI Platform
https://github.com/GoogleCloudPlatform/vertex-ai-samples

#### Hyperparameter tunning
```python
hpt = hypertune.HyperTune()  
hpt.report_hyperparameter_tuning_metric(hyperparameter_metric_tag=’loss’, metric_value=loss, global_step=epochs)
```

```yaml
trainingInput:
  scaleTier: CUSTOM
  masterType: complex_model_m
  workerType: complex_model_m
  parameterServerType: large_model
  workerCount: 9
  parameterServerCount: 3
  hyperparameters:
    goal: MAXIMIZE
    hyperparameterMetricTag: loss
    maxTrials: 30
    maxParallelTrials: 1
    enableTrialEarlyStopping: True
    params:
    - parameterName: hidden1
      type: INTEGER
      minValue: 40
      maxValue: 400
      scaleType: UNIT_LINEAR_SCALE
    - parameterName: numRnnCells
      type: DISCRETE
      discreteValues:
      - 1
      - 2
      - 3
      - 4
    - parameterName: rnnCellType
      type: CATEGORICAL
      categoricalValues:
      - BasicLSTMCell
      - BasicRNNCell
      - GRUCell
      - LSTMCell
      - LayerNormBasicLSTMCell
```


#### Package the script into a docker container
```docker
FROM gcr.io/deeplearning-platform-release/base-cpu
RUN pip install -U fire cloudml-hypertune scikit-learn==0.20.4 pandas==0.24.2
WORKDIR /app
COPY train.py .
ENTRYPOINT ["python", "train.py"]
```

#### BigQuery
```bash
bq query \
-n 0 \
--destination_table covertype_dataset.validation\
--replace \
--use_legacy_sql=false \
'SELECT * \
FROM `covertype_dataset.covertype` as cover \
WHERE \
MOD(ABS(FARM_FINGERPRINT(TO_JSON_STRING(cover))), 10) = 0'
```

```bash
bq extract \
--destination_format CSV \
covertype_dataset.validation \
$VALIDATION_FILE_PATH
```

#### KubeFlow prebuilt components
```python
import kfp
URI = 'https://raw.githubusercontent.com/kubeflow/pipelines/0.2.5/components/gcp/'
component_store = kfp.components.ComponentStore(local_search_paths=None, url_search_prefixes=[URI])
bigquery_query_op = component_store.load_component('bigquery/query')
mlengine_train_op = component_store.load_component('ml_engine/train')
mlengine_deploy_op = component_store.load_component('ml_engine/deploy')
```

```python
    create_testing_split = bigquery_query_op(
        query=query,
        project_id=project_id,
        datset_id=dataset_id,
        table_id='',
        output_gcs_path=testing_file_path,
        dataset_location=dataset_location,
    ) # TO DO: Use the bigquery_query_op
```

kfp submit run parameters should be in the same order as the function paramters	