# Docker

## Install docker
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

To run docker commands without sudo
```bash
sudo usermod -aG docker your-user
```

!!! note
    You should logout and log back in in order for this to take effect, otherwise run commands with sudo

## Access gcr (google cloud registery, where our docker images live :p)
If you are inside a gcp vm you don't have to log in to your google acount an application credentials is provided and will be utilized by default
```bash
gcloud auth configure-docker
```

## Install gcloud for Ubuntu
```bash
export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)"
echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update && sudo apt-get install google-cloud-sdk
```

## Initialize configurations
like login account and default project
```bash
gcloud init

```
## Portainer
Portainer is a dashboard for docker
```bash
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```
