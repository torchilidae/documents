
This guide outlines how to install a high-availability K3s cluster using `kube-vip` for virtual IP failover and `MetalLB` for load balancing within the cluster.

---

## 1. Install K3s (First Control Plane Node)

Disable the default service load balancer (`servicelb`) and Traefik ingress controller.

```sh
curl -fL https://get.k3s.io | sh -s - server \
  --tls-san 10.10.1.4 \
  --disable traefik \
  --disable servicelb \
  --cluster-init
```

> `--tls-san` must match your kube-vip virtual IP.

---

## 2. Deploy kube-vip Static Pod via Manifest

Install the necessary RBAC roles:

```sh
curl https://kube-vip.io/manifests/rbac.yaml -o /var/lib/rancher/k3s/server/manifests/kube-vip-rbac.yaml
```

---

## 3. Create kube-vip Manifest

Set environment variables and generate the kube-vip manifest:

```sh
export VIP=10.10.1.4
export INTERFACE=enp0s31f6
export KVVERSION=$(curl -sL https://api.github.com/repos/kube-vip/kube-vip/releases | jq -r ".[0].name")

alias kube-vip="ctr image pull ghcr.io/kube-vip/kube-vip:$KVVERSION; \
  ctr run --rm --net-host ghcr.io/kube-vip/kube-vip:$KVVERSION vip /kube-vip"

kube-vip manifest daemonset \
  --interface $INTERFACE \
  --address $VIP \
  --inCluster \
  --taint \
  --controlplane \
  --services \
  --arp \
  --leaderElection > /var/lib/rancher/k3s/server/manifests/kube-vip-daemonset.yaml
```

Save the generated manifest to `/var/lib/rancher/k3s/server/manifests/` so it is automatically deployed by K3s.

---

## 4. Join Additional Control Plane Nodes

Use the same virtual IP and disable default addons.

```sh
curl -fL https://get.k3s.io | sh -s - server \
  --token=TOKEN_ID \
  --tls-san 10.10.1.4 \
  --disable servicelb \
  --disable traefik \
  --server https://10.10.1.5:6443
```

The location of the TOKEN_ID can be found in the `/var/lib/rancher/k3s/server/node-token` file.

---

## 5. Install MetalLB (Native Mode)

Install MetalLB for providing LoadBalancer services to workloads:

```sh
# kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/main/config/manifests/metallb-native.yaml
curl https://raw.githubusercontent.com/metallb/metallb/main/config/manifests/metallb-native.yaml -o /var/lib/rancher/k3s/server/manifests/metallb-native.yaml
```

---
## 6. Setup IP Pool manifests for MetalLB

Create a file named `metallb-config.yaml` with the following content:

```sh
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: metallb-pool
  namespace: metallb-system
spec:
  addresses:
    - 10.10.1.20-10.10.1.100
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: metallb-l2
  namespace: metallb-system
spec:
  ipAddressPools:
    - metallb-pool
```
Apply this configuration to your cluster:
```sh
kubectl apply -f metallb-config.yaml
```

---

## 7. Incrase pods limit on each node

create a file `/etc/rancher/k3s/config.yaml` with the following content in each node:

```yaml
kubelet-arg:
  - max-pods=250
```
after that run the following command:
```sh
sudo systemctl restart k3s
```
the pods limit can be validated by running the following command:
```sh
kubectl get nodes -o json | jq '.items[] | {name: .metadata.name, maxPods: .status.allocatable["pods"]}'
```

---

## 8. Install cert-manager

Install cert-manger using helm chart:

```sh
helm repo add jetstack https://charts.jetstack.io --force-update

helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.18.1 \
  --set crds.enabled=true
```

---