## 1. Creating Experiments
```python
# return string ID of experiment
mlflow.create_experiment(
	name: str,
	artifact_location: None,
	tags: None
)
```

We can create multiple experiments but only the one that has 'active' status works, we can set the status as active through the following method:
```python
# return an Experiment object
mlflow.set_experiment(experiment_name: str, experiment_id: str)
```

## 2. Retrieving Experiments

1. Retrieving by name:
```python
# return an Experiment object (None otherwise)
mlflow.get_experiment_by_name(name: str)
```

2. Retrieving by id:
```python
mlflow.get_experiment(experiment_id: str)
```


## 3. Updating Experiments

```python
mlflow.set_experiment_tag(key: str, value: Any)
mlflow.set_experiment_tags(tasg: dict)
# client
client.rename_experiment(experiment_id: str, new_name: str)
```


## 4. Deleting Experiments
```python
client.delete_experiment(experiment_id: str)
client.restore_experiment(experiment_id: str)
```

## 5. Creating Run
```python
run = mlflow.start_run()
run = mlflow.get_run(run_id)

run = client.create_run(experiment_id=)
```

