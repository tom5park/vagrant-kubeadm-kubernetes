Vagrant.configure("2") do |config|

    config.vm.provision "shell" do |s|
      ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
      s.inline = <<-SHELL
        echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
        echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
        apt-get update -y
        echo "10.1.1.71  master-node" >> /etc/hosts
        echo "10.1.1.72  worker-node01" >> /etc/hosts
        echo "10.1.1.72  worker-node02" >> /etc/hosts
      SHELL
    end

    config.vm.define "master" do |master|
      master.vm.box = "ubuntu/focal64"
      master.vm.hostname = "master-node"
      master.vm.network "public_network", bridge: "en0: ethernet", ip: "10.1.1.71"
      master.vm.provider "virtualbox" do |vb|
          vb.memory = 2048
          vb.cpus = 2
          vb.gui = false
      end
      master.vm.provision "shell", path: "scripts/common.sh"
      master.vm.provision "shell", path: "scripts/master.sh"
    end

    (1..2).each do |i|

    config.vm.define "node0#{i}" do |node|
      node.vm.box = "ubuntu/focal64"
      node.vm.hostname = "worker-node0#{i}"
      node.vm.network "public_network", bridge: "en0: ethernet", ip: "10.1.1.7#{i + 1}"
      node.vm.provider "virtualbox" do |vb|
          vb.memory = 2048
          vb.cpus = 2
          vb.gui = false
      end
      node.vm.provision "shell", path: "scripts/common.sh"
      node.vm.provision "shell", path: "scripts/node.sh"
    end

    end
  end