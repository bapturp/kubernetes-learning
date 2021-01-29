# Rolling update on Kubernetes

* k8s installed with kubeadm and apt-get


## Abstract

__Upgrade control planes Nodes__
- Check available version `apt-get update && apt-cache showpkg kubeadm`
- Upgrade kubeadm: `apt-mark unhold kubeadm && apt-get update && apt-get install -y kubeadm=1.18.0 && apt-mark hold kubeadm`
- Drain node `kubectl drain master --ignore-daemonsets`
- Check the upgrade feasibility `kubeadm upgrade plan`
- Run the kubeadm upgrade `kubeadm upgrade apply`
- Uncordon the controle plane node: `kubectl uncordon master1`
- Upgrade additional control plane node: `kubeadm upgrade master`
- upgrade kubelet and kubectl: `apt-mark unhold kubelet kubectl && apt-get update && apt-get install -y kubelet=1.18.0 kubectl=1.18.0 && apt-mark hold kubelet kubectl`
- 

__Upgrade worker nodes__
- Upgrade kubeadm binaries: `apt-mark unhold kubeadm && apt-get update && apt-get install -y kubeadm=1.18.0 && apt-mark hold kubeadm`
- Drain node node `kubectl drain master --ignore-daemonsets`
- Upgrade kubeadm `kubeadm upgrade node`
- Upgrade kubelet and kubectl: `apt-mark unhold kubelet kubectl && apt-get update && apt-get install -y kubelet=1.18.0 kubectl=1.18.0 && apt-mark hold kubelet kubectl`
