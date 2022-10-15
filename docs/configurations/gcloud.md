# Cheat sheet
## create
```
gcloud compute instances start swarm-manager
```
## set-machine-type

Before set-machine-type
```
gcloud config set compute/zone us-central1-a
```
Instances should be stopped
```
gcloud compute instances set-machine-type swarm-manager --machine-type g1-small
```
## stop
```
gcloud compute instances stop swarm-manager
```
## Completion makes life easier, just use it
```
eval $(gcloud completion zsh)
```