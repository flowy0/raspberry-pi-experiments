# Steps


## Overview 
1. Flash Raspberry PI Image
2. Install Docker
3. Setup Home Assistant via Docker Compose
3. Install k3s (WIP)

## Use Raspberry PI Imager to flash SD Card 
- Use advanced settings to setup host name, account and ssh access

## Install Docker

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```


```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

sudo docker run hello-world


Do Post Linux Install Steps - https://docs.docker.com/engine/install/linux-postinstall/

```
#add current user to docker group
sudo usermod -aG docker $USER

# refresh the changes
newgrp docker

#test 
docker run hello-world

```

Set auto start of services
```
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```




Reboot pi `sudo shutdown -r now`



Fix this error 
-bash: warning: setlocale: LC_ALL: cannot change locale (en_SG.UTF-8)

Refer to https://jeffreytse.net/computer/2020/10/19/fix-warning-setlocale-cannot-change-locale.html


## Install K3s 

Update /boot/firmware/cmdline.txt
https://docs.k3s.io/installation/requirements?os=pi

append `cgroup_memory=1 cgroup_enable=memory` to `/boot/firmware/cmdline.txt`

Use quick start script 

curl -sfL https://get.k3s.io | sh -


Deploy K8s Dashboard via Helm

Install Helm 
```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```


Copy the kubeconfig from sudo nano /etc/rancher/k3s/k3s.yaml to your local machine kubeconfig and edit the server name from 

`server: https://127.0.0.1:6443`
to 
`server: https://<your-pi-ip>:6443`

You can access the k3s cluster from your machine now 

Deploy K8s Dashboard using your machine, see https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

```
# Add kubernetes-dashboard repository
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
# Deploy a Helm Release named "kubernetes-dashboard" using the kubernetes-dashboard chart
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```


Port Forward and Access via https://localhost:8443

```
# Port forward for now, ingress to be setup later.
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
```


###  Links
GPG Error: https://forums.raspberrypi.com/viewtopic.php?t=361409
