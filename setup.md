# Steps


## Overview 
1. Flash Raspberry PI Image
2. Install Docker
3. Setup Home Assistant via Docker Compose
3. Install k3s (WIP)

## Use Raspberry PI Imager to flash SD Card 
- Use advanced settings to setup host name, account and ssh access

## Basic Setup 
Vim, Docker, Bashrc - See https://github.com/flowy0/UsefulStuff/blob/main/setup_debian.md

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
