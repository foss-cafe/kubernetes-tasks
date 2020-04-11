# Steps to Migrate the cluster using kubeadm

### upgrade workflow at high level is the following:

1. Upgrade the primary control plane node.
2. Upgrade additional control plane nodes.
3. Upgrade worker nodes.

### Process

1. check kubeadm version

```bash
kubeadm version
```

unhold kubeadm nad kubelet

```bash
apt-mark unhold kubeadm kubelet
```

Upgrade kubeadm to latest

```bash
apt-get update && apt-get install -y kubeadm=1.18.x-00 kubelet=1.18.x-00
apt-mark hold kubeadm kubelet
kubeadm version
```

2. Drain control plane node

```bash
kubectl drain <cp-node-name> --ignore-daemonsets
kubectl get nodes
```

3. Plan upgrade

```bash
sudo kubeadm upgrade plan
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[preflight] Running pre-flight checks.
[upgrade] Running cluster health checks
[upgrade] Fetching available versions to upgrade to
[upgrade/versions] Cluster version: v1.17.3
[upgrade/versions] kubeadm version: v1.18.0
[upgrade/versions] Latest stable version: v1.18.0
[upgrade/versions] Latest version in the v1.17 series: v1.18.0

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   CURRENT             AVAILABLE
Kubelet     1 x v1.17.3   v1.18.0

Upgrade to the latest version in the v1.17 series:

COMPONENT            CURRENT   AVAILABLE
API Server           v1.17.3   v1.18.0
Controller Manager   v1.17.3   v1.18.0
Scheduler            v1.17.3   v1.18.0
Kube Proxy           v1.17.3   v1.18.0
CoreDNS              1.6.5     1.6.7
Etcd                 3.4.3     3.4.3-0

You can now apply the upgrade by executing the following command:

    kubeadm upgrade apply v1.18.0

_____________________________________________________________________
```

upgrade

```bash
sudo kubeadm upgrade apply v1.18.0
----
[upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.18.0". Enjoy!

[upgrade/kubelet] Now that your control plane is upgraded, please proceed with upgrading your kubelets if you haven't already done so.
```

Upgrade your CNI based on the provider that you choose for your cluster

4. Make control plane node scheduleable

```bash
kubectl uncordon <cp-node-name>
kubectl get nodes
```

5. Upgrade kubelet

```bash
systemctl restart kubelet
```

## Upgrade worker nodes

1. Drain the nodes
2. upgrade kubeadm and kubelet
3. perform upgrade and uncordon the node

```bash
kubectl uncordon <node-to-drain>
```

```bash
sudo kubeadm upgrade node --dry-run # validation
sudo kubeadm upgrade node
sudo systemctl restart kubelet
```
