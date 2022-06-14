# Kubernets

## Install
1. Minikube: `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mkdir -p /usr/local/bin/ && sudo mv minikube /usr/local/bin/minikube && sudo ln -s /bin/minikube /usr/local/bin/minikube`
2. Kubectl: `curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl && chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl && sudo ln -s /usr/local/bin/kubectl /bin/kubectl`
3. Autocomplete kubectl: `echo "if [ $commands[kubectl] ]; then source <(kubectl completion zsh); fi" >> ~/.zshrc` or `echo "source <(kubectl completion bash)" >> ~/.bashrc`
4. Helm: `wget --continue https://get.helm.sh/helm-v3.0.3-linux-amd64.tar.gz && tar xvf helm-v3.0.3-linux-amd64.tar.gz && cd linux-amd64 && chmod +x ./helm && sudo mv helm /usr/local/bin && sudo ln -s /usr/local/bin/helm /bin/helm`
j
### Install seldon-core
1. `kubectl create namespace seldon-system`
2. `helm install seldon-core seldon-core-operator --repo https://storage.googleapis.com/seldon-charts --set usageMetrics.enabled=true --namespace seldon-system`

##### Seldon core analytics
Seldon Core provides an example Helm analytics chart that displays the above Prometheus metrics in Grafana. You can install it with:
```bash
helm install seldon-core-analytics seldon-core-analytics \
   --repo https://storage.googleapis.com/seldon-charts \
   --set grafana_prom_admin_password=password \
   --set persistence.enabled=false \
   --namespace seldon-system
```
The available parameters are:

- `grafana_prom_admin_password` : The admin user Grafana password to use.
- `persistence.enabled` : Whether Prometheus persistence is enabled.
Once running you can expose the Grafana dashboard with:
```bash
kubectl port-forward $(kubectl get pods -n seldon-system -l app=grafana-prom-server -o jsonpath='{.items[0].metadata.name}') 3000:3000 -n seldon-system
```
You can then view the dashboard at http://localhost:3000/dashboard/db/prediction-analytics?refresh=5s&orgId=1

### Install ambassador
#### From seldon.io
1. `helm repo add stable https://kubernetes-charts.storage.googleapis.com/`
2. `helm repo update`
3. `helm install ambassador stable/ambassador --set crds.keep=false`

#### From official website
1. `helm repo add datawire https://www.getambassador.io`
2. `kubectl create namespace ambassador`
3. `helm install --name ambassador --namespace ambassador datawire/ambassador`



### Install source-to-image
1. `wget --continue https://github.com/openshift/source-to-image/releases/download/v1.2.0/source-to-image-v1.2.0-2a579ecd-linux-amd64.tar.gz`
2. `tar xvf source-to-image-v1.2.0-2a579ecd-linux-amd64.tar.gz`
3. `sudo cp s2i sti /usr/local/bin/`
4. `sudo ln -s /usr/local/bin/s2i /bin/s2i`
4. `sudo ln -s /usr/local/bin/sti /bin/sti`
