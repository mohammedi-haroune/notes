# Anaconda

## Create an environment
```bash
conda create -n env36 python=3.6
```

## Activate an environment
```bash
conda activate env36
```

## Disable (base) on the terminal
```bash
conda config --set auto_activate_base False
```

## Use pip
1. Run `conda install pip`. This will install pip to your venv directory.
2. Install new packages by doing `/anaconda/envs/venv_name/bin/pip install package_name`.
