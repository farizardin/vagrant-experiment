- Install Vagrant
- Install libvirt + dependencies
- Install vagrant-libvirt
- Install kubectl client only for local machine
- Install kubernetes-dashboard
- setup admin-user.yaml for dashboard
- kubectl apply -f admin-user.yaml --validate=false
- kubectl -n kubernetes-dashboard create token admin-user

get master node token:
sudo cat /var/lib/rancher/k3s/server/node-token

worker node setup:
curl -sfL https://get.k3s.io | sudo \
K3S_URL="<master node ip>:6443" \
K3S_TOKEN="<paste-master-node-token-here>" \
INSTALL_K3S_EXEC="agent --node-ip=<current node ip>" sh -

local setup:
- get master node config
sudo cat /etc/rancher/k3s/k3s.yaml

- paste the config to local machine
mkdir -p ~/.kube
nano ~/.kube/config

