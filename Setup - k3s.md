Additional installation steps for Rancher on Raspberry Pi
---
* Setup the ip table and boot file
```Shell
sudo iptables -F
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
sudo reboot
```
* Update the "/boot/cmdline.txt"
```Shell
sudo awk '{print $0 " cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1"}' /boot/firmware/cmdline.txt > tmp && chmod 755 tmp && mv -f tmp /boot/firmware/cmdline.txt
cat /boot/firmware/cmdline.txt
ls -l /boot/firmware/cmdline.txt
```
Initialize rancher on master server
---
```Shell
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --cluster-init" sh -s -
```
Get token from the master server for adding other servers
---
```Shell
cat /var/lib/rancher/k3s/server/node-token
```
Join the master server with the token
---
```Shell
curl -sfL https://get.k3s.io | K3S_TOKEN=<Token Value> INSTALL_K3S_EXEC="server --server https://<IP address of Master>:6443" sh -s -
```
Join the worker nodes to the Kubernetes cluster
---
```Shell
curl -sfL https://get.k3s.io | K3S_URL="https://<IP address of Master>:6443" K3S_TOKEN=${Token_Value}$ sh -
```
Export the KUBE config file to the system
---
```Shell
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```
Install Helm and Cert Manager
---
```Shell
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
kubectl create namespace cattle-system
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.5.1/cert-manager.crds.yaml
```
Install Rancher
---
```Shell
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.5.1
helm install rancher rancher-latest/rancher --namespace cattle-system --set hostname=vm-ubuntu.local --set bootstrapPassword=admin
```
Uninstall K3S
---
```Shell
/usr/local/bin/k3s-uninstall.sh
```
