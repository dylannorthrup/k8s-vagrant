NUM_NODES = 4
NUM_CONTROLLER_NODE = 1
IP_NTW = "192.168.56."
CONTROLLER_IP_START = 10
NODE_IP_START = 20

Vagrant.configure("2") do |config|
  config.vm.box = "rockylinux/8"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end

  (1..NUM_NODES).each do |i|
    config.vm.define "node0#{i}" do |node|
      node.vm.provider "virtualbox" do |vb|
        vb.check_guest_additions = false
        vb.customize ["modifyvm", :id, "--firmware", "efi"]
        vb.name = "node0#{i}"
        vb.memory = 2048
        vb.cpus = 2
      end

      node.vm.hostname = "node0#{i}"
      node.vm.network "private_network", ip: IP_NTW + "#{NODE_IP_START + i}"
      node.vm.network "forwarded_port", guest: 22, host: "#{2720 + i}"
    end
  end

  i = 0

  (1..NUM_CONTROLLER_NODE).each do [i]
    config.vm.define "controller" do |node|
      node.vm.provider "virtualbox" do |vb|
        vb.check_guest_additions = false
        vb.customize ["modifyvm", :id, "--firmware", "efi"]
        vb.name = "controller"
        vb.memory = 2048
        vb.cpus = 2
      end

      node.vm.hostname = "controller"
      node.vm.network "private_network", ip: IP_NTW + "#{CONTROLLER_IP_START + i}"
      node.vm.network "forwarded_port", guest: 22, host: "#{2710 + i}"
    end
  end
end
