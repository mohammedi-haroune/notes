## Concepts
a _Run_ records:
- Code Version
- Start & End time
- Source (the file used to launch, or project)
- Parameters
- Metrics
- Artifacts

A set of Runs could be organized into an _Experiment_

## Where Runs Are Recorded
- Local file path specified as `file:/my/local/dir`
- Database encoded as `<dialect>+<driver>://<username>:<password>@<host>:<port>/<database>`
- HTTP server specified as `https://my-server:5000`
- Databricks workspace 

## Logging Data to Runs

- `mlflow.set_tracking_uri()` connects to a tracking URI. You can also set the `MLFLOW_TRACKING_URI` (See above for "Where Runs Are Recorded")
- `mlflow.tracking.get_tracking_uri()` returns the current tracking URI.
-  `mlflow.create_experiment()`  creates a new experiment and returns its ID. Runs can be launched under the experiment by passing the experiment ID to `mlflow.start_run`.
- `mlflow.set_experiment()` sets an experiment as active. If the experiment does not exist, creates a new experiment. If you do not specify an experiment in `mlflow.start_run()`, new runs are launched under this experiment.
- `mlflow.start_run()` returns the currently active run (if one exists), or starts a new run and returns a mlflow.ActiveRun object usable as a context manager for the current run. You do not need to call start_run explicitly: calling one of the logging functions with no active run automatically starts a new one.

- `mlflow.end_run()` ends the currently active run, if any, taking an optional run status.

- `mlflow.active_run()` returns a mlflow.entities.Run object corresponding to the currently active run, if any.

- `mlflow.log_param()` logs a single key-value param in the currently active run. The key and value are both strings. Use mlflow.log_params() to log multiple params at once.

- `mlflow.log_metric()` logs a single key-value metric. The value must always be a number. MLflow remembers the history of values for each metric. Use mlflow.log_metrics() to log multiple metrics at once.

- `mlflow.set_tag()` sets a single key-value tag in the currently active run. The key and value are both strings. Use mlflow.set_tags() to set multiple tags at once.

- `mlflow.log_artifact()` logs a local file or directory as an artifact, optionally taking an artifact_path to place it in within the run’s artifact URI. Run artifacts can be organized into directories, so you can place the artifact in a directory this way.

- `mlflow.log_artifacts()` logs all the files in a given directory as artifacts, again taking an optional artifact_path.

- `mlflow.get_artifact_uri()` returns the URI that artifacts from the current run should be logged to.

