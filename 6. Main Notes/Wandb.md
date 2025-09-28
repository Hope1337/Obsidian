## Fundamental

```python
import wandb

wandb.login()

run = wandb.init(
	project = "project-name",
	notes = "this is the project of ...",
	tags = ["sekiro", "elden ring"]
	config = {
		'epochs': 100,
		'learning_rate': 0.001,
		'batch_size': 128
	}
)

run.log({"accuracy": , "loss":})
run.log_artifact("path_to_model.onnx", name="trained_model", type="model")
```

You can update or add new param after `wandb.init`:
```python
run.config.undate({'lr': 0.1, 'channels':16})
run.config['batch_size'] = 32
```

`wandb` will automatically pickup config in the file `config-defaults.yaml` in the same directory as your run scripts. If you want to use config file other than that, use `--configs` flag:
```bash
python train.py --configs other-config.yaml
```
