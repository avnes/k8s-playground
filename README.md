# Kubernetes playground using k0s and k0sctl

## Pre-requisites

- Make sure your user has password-less sudo configured.
- Make sure sshd service is started: `sudo systemctl start sshd.service`.

## Create cluster

```bash
export K0S_CLUSTER=playground

k0sctl init --k0s --cluster-name ${K0S_CLUSTER} --user $LOGNAME localhost > k0sctl-${K0S_CLUSTER}.yaml
sed -i 's/^.*role: controller$/    role: "controller+worker"/g' k0sctl-${K0S_CLUSTER}.yaml
k0sctl apply --config k0sctl-${K0S_CLUSTER}.yaml
```

## Create kubeconfig

```bash
export K0S_CLUSTER=playground

mkdir -p ~/.kube
k0sctl kubeconfig --config k0sctl-${K0S_CLUSTER}.yaml > ~/.kube/${K0S_CLUSTER}.config
chmod 600 ~/.kube/${K0S_CLUSTER}.config
export KUBECONFIG=~/.kube/${K0S_CLUSTER}.config
```
