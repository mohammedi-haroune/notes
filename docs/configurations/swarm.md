# Docker Swarm

## What is a swarm?
A swarm consists of multiple Docker hosts which run in swarm mode and act as managers (to manage membership and delegation) and workers (which run swarm services).

A key difference between standalone containers and swarm services is that only swarm managers can manage a swarm

## Nodes
A node is an instance of the Docker engine participating in the swarm. You can also think of this as a Docker node. You can run one or more nodes on a single physical computer or cloud server.

To deploy your application to a swarm, you submit a service definition to a manager node. The manager node dispatches units of work called tasks to worker nodes.

## Services and tasks
A **service** is the definition of the tasks to execute on the manager or worker nodes.

A **task** carries a Docker container and the commands to run inside the container. It is the atomic scheduling unit of swarm.

## Load balancing
The swarm manager uses **ingress load balancing** to expose the services you want to make available externally to the swarm.

Swarm mode has an internal DNS component that automatically assigns each service in the swarm a DNS entry.


## docker-machine
```bash
export GOOGLE_DISK_SIZE=10
export GOOGLE_DISK_TYPE=pd-standard
export GOOGLE_MACHINE_IMAGE=https://www.googleapis.com/compute/v1/projects/ubuntu-os-cloud/global/images/ubuntu-1804-bionic-v20190320
export GOOGLE_MACHINE_TYPE=g1-small
export GOOGLE_NETWORK=default
export GOOGLE_PREEMPTIBLE=true
export GOOGLE_PROJECT=amanee-ec1eb
export GOOGLE_ZONE=us-central1-a
export GOOGLE_USERNAME=docker
export GOOGLE_USE_EXISTING=true # try to find existing one, otherwise fail
# export GOOGLE_USE_INTERNAL_IP=
# export GOOGLE_ADDRESS=
# export GOOGLE_SUBNETWORK=
# export GOOGLE_TAGS=
```

restarting a google cloud with ephemeral ip address cuases docker-machine to fail, running `docker-machine regenerate-certs swarm-worker1` fixes the issue but it restarts the docker daemon which might stop running containers.

restarting also causes swarm to be broken, we need to `leave` and re`join` the nodes.

## get the token
```bash
docker swarm join-token worker
```
Response:
```bash
docker swarm join --token SWMTKN-1-29na4ee9gsnkt0zn6artrtx2bgb2ajeev96k1bhxdxo7xl5gpi-8o8uf1thlf5c31qrrosuppn7d 10.128.0.8:2377
```

## inspect service
```bash
docker service inspect --pretty <SERVICE-ID>
```
To see which nodes are running the service:
```bash
docker service ps <SERVICE-ID>
```

Run `docker ps` on the node where the task is running to see details about the container for the task.

instances doens't have access to the gcr, this is rather a weired behaviour.


## Docker swarm

### Init
```bash
docker swarm init
```
### Deploy
```bash
docker stack deploy -c docker-compose.yml <stack-name>
```
### Remove
```bash
docker stack rm <stack-name>
```
### Scale
```bash
docker service scale <service-name>=<number>
```
## Configure a docker-machine shell to the swarm manager
So far, you’ve been wrapping Docker commands in docker-machine ssh to talk to the VMs. Another option is to run `docker-machine env <machine>` to get and run a command that configures your current shell to talk to the Docker daemon on the VM. This method works better for the next step because it allows you to use your local docker-compose.yml file to deploy the app “remotely” without having to copy it
