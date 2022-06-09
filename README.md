# Kubernetes playground using k0s and k0sctl

:warning: This whole guide has only been tested on linux-x64. If issues are found on other architectures, then please raise a pull request.

## Prerequisite

- Make sure your user or group has password-less sudo configured through /etc/sudoers. Linux example: `%wheel  ALL=(ALL)       NOPASSWD: ALL`
- Make sure sshd service is started: `sudo systemctl start sshd.service`.
- Make sure you have a SSH key pair. If you do not have a key pair, then you can generate it with: `ssh-keygen`

## Install k0sctl

```bash
K0S_ARCH=linux-x64 # OR: darwin-arm64, darwin-x64, linux-arm, linux-arm64
sudo curl -L https://github.com/k0sproject/k0sctl/releases/download/v0.13.0/k0sctl-$K0S_ARCH -o /usr/local/bin/k0sctl
sudo chmod +x /usr/local/bin/k0sctl
k0sctl version
```

## Create cluster

```bash
export K0S_CLUSTER=playground

k0sctl init --k0s --cluster-name ${K0S_CLUSTER} --user $(logname) localhost > k0sctl-${K0S_CLUSTER}.yaml # Or replace 'localhost' with an actual hostname or IP address
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
