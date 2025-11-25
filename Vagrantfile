Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"
  config.ssh.insert_key = false

  config.vm.provider :libvirt do |libvirt|
    libvirt.memory = 2048
    libvirt.cpus = 2
  end

  # Master Node
  config.vm.define "k3s-master" do |master|
    master.vm.hostname = "k3s-master"
    master.vm.network "private_network", ip: "192.168.56.10"

    master.vm.provision "shell", inline: <<-SHELL
      echo "[MASTER] Installing k3s server..."
      curl -sfL https://get.k3s.io | \
        INSTALL_K3S_EXEC="server --node-ip=192.168.56.10" sh -

      echo "[MASTER] Token:"
      sudo cat /var/lib/rancher/k3s/server/node-token
    SHELL
  end

  # Worker 1 (no k3s agent)
  config.vm.define "k3s-worker-1" do |worker|
    worker.vm.hostname = "k3s-worker-1"
    worker.vm.network "private_network", ip: "192.168.56.11"
  end

  # Worker 2 (no k3s agent)
  config.vm.define "k3s-worker-2" do |worker|
    worker.vm.hostname = "k3s-worker-2"
    worker.vm.network "private_network", ip: "192.168.56.12"
  end
end
