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

Update `/boot/firmware/cmdline.txt` ,See https://docs.k3s.io/installation/requirements?os=pi

append `cgroup_memory=1 cgroup_enable=memory` to `/boot/firmware/cmdline.txt`

Use quick start script 

`curl -sfL https://get.k3s.io | sh -`

## Check k3s is running 


The error is due to file permissions.
```bash
g@pi3:~ $ kubectl get nodes
WARN[0000] Unable to read /etc/rancher/k3s/k3s.yaml, please start server with --write-kubeconfig-mode or --write-kubeconfig-group to modify kube config permissions
error: error loading config file "/etc/rancher/k3s/k3s.yaml": open /etc/rancher/k3s/k3s.yaml: permission denied
g@pi3:~ $ sudo kubectl get nodes
NAME   STATUS   ROLES           AGE   VERSION
pi3    Ready    control-plane   53s   v1.34.4+k3s1
```



 See fix at [Stack Exchange](https://devops.stackexchange.com/questions/16043/error-error-loading-config-file-etc-rancher-k3s-k3s-yaml-open-etc-rancher)


```bash
export KUBECONFIG=~/.kube/config
mkdir ~/.kube 2> /dev/null
sudo k3s kubectl config view --raw > "$KUBECONFIG"
chmod 600 "$KUBECONFIG"
```


# Update kubeconfig from your machine

Copy the kubeconfig from sudo nano /etc/rancher/k3s/k3s.yaml to your local machine kubeconfig and edit the server name from 

`server: https://127.0.0.1:6443`
to 
`server: https://<your-pi-ip>:6443`

You can access the k3s cluster from your machine now 


## Add a new agent node
Get the token from the 1st node
`sudo cat /var/lib/rancher/k3s/server/node-token`

in the agent node machine:
`export mynodetoken=yourtoken`

run 
```
curl -sfL https://get.k3s.io | \
K3S_URL=https://machine-ip:6443 \
K3S_TOKEN=$mynodetoken \
sh -
```

### Check for Status

You should see something like this
```
g@pi3:~ $ kubectl get nodes
NAME   STATUS   ROLES           AGE   VERSION
pi3    Ready    control-plane   68m   v1.34.4+k3s1
pi4    Ready    <none>          95s   v1.34.4+k3s1
```


