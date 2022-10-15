# Basics
Don't confuse **Local** and **Standalone** modes. 

Local mode is a dev mode that runs everything in the same JVM, it's the default mode and configured with master `local[*]` or `local[n]`

Standalone mode is Spark's own cluster manager just like (not that much) YARN and Mesos

# Standalone
- Start the master node
```bash
./start-master.sh
```
- Start the worker node (could be in the same machine)
```bash
./start-worker.sh spark://mac.local:7077
```
- In your application set the master to `spark://mac.local:7077`
- We can adjust how much executors per worker and how much cores an executor allocates with  `spark.executor.cores`  and `spark.cores.max`
- `spark.executor.instances` has no effect in the standalone mode
- Writing to the filesystem asks all the workers to write their partitions to the specified path, so we have to make sure that path exists for all workers

# S3

- Set `spark.jars.packages` to `com.amazonaws:aws-java-sdk:1.12.310,org.apache.hadoop:hadoop-aws:3.3.4`
- If the S3 bucket is public set `fs.s3a.aws.credentials.provider` to `org.apache.hadoop.fs.s3a.AnonymousAWSCredentialsProvider`
- Read the S3 object (not sure about the bucket) using its URL prefixed by `s3a://`. Example `s3a://cellpainting-gallery/datasets/dataset.csv`

# Kubernetes
```bash
brew install openjdk@11
brew install apache-spark
brew install ubuntu/
```


```bash
microk8s enable registry
```

Since `microk8s` is running in a VM, we need to use the VM IP address to talk to the registry from the host machine (Mac OS), but we need to use the registry service cluster IP (or DNS name ?) when submitting Spark applications

```bash
# Get the VM IP
kubectl get nodes -o wide

# add "insecure-registries": ["http://192.168.64.2:32000"] to ~/.docker/daemon.json
# or via the Docker app, then restart
```

```bash
cd /opt/homebrew/Cellar/apache-spark/3.2.1/libexec/

docker-image-tool.sh -r 192.168.64.2:32000 build

docker-image-tool.sh -r 192.168.64.2:32000 -p ./kubernetes/dockerfiles/spark/bindings/python/Dockerfile build

docker-image-tool.sh -r 192.168.64.2:32000 push
```
