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

- start kubernetes dashboard
nohup sudo kubectl -n kubernetes-dashboard port-forward --address=0.0.0.0 svc/kubernetes-dashboard 8443:443 > dashboard.log 2>&1 &

or

- auto start
sudo nano /etc/systemd/system/k8s-dashboard-portforward.service

paste this:
[Unit]
Description=K3s Dashboard Port Forward
After=network.target

[Service]
User=root
ExecStart=/usr/local/bin/kubectl -n kubernetes-dashboard port-forward --address=0.0.0.0 svc/kubernetes-dashboard 8443:443
Restart=always

[Install]
WantedBy=multi-user.target

save, then:
sudo systemctl daemon-reload
sudo systemctl enable --now k8s-dashboard-portforward
