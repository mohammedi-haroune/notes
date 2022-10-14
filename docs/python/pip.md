# Pip

## Install packages without internet
If you want to install a bunch of dependencies from, say a requirements.txt, you would do:

```bash
mkdir dependencies
pip download -r requirements.txt -d "./dependencies"
tar cvfz dependencies.tar.gz dependencies
```
And, once you transfer the dependencies.tar.gz to the machine which does not have internet you would do:

```bash
tar zxvf dependencies.tar.gz
cd dependencies
pip install * -f ./ --no-index
```

## Use pip
