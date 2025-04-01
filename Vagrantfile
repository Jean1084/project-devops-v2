Vagrant.configure("2") do |config|
    # Define the base box
    config.vm.box = "ubuntu/focal64"
  
    # Define common VM settings
    nodes = [
      { name: "master", ip: "192.168.56.10" },
      { name: "wordpress", ip: "192.168.56.11" },
      { name: "mysql", ip: "192.168.56.12" },
      { name: "registry", ip: "192.168.56.13" }
    ]
  
    nodes.each do |node|
      config.vm.define node[:name] do |node_config|
        node_config.vm.hostname = node[:name]
        node_config.vm.network "private_network", ip: node[:ip]
  
        node_config.vm.provider "virtualbox" do |vb|
          vb.memory = "1024" # 1 GB RAM
          vb.cpus = 1        # 1 CPU
          vb.customize ["modifyvm", :id, "--hddsize", 30720] # 30 GB Disk
        end
        config.vm.synced_folder ".", "/home/vagrant/workspace", type: "virtualbox"
        config.vm.provision "shell", inline: <<-SHELL
          ln -sf /home/vagrant/workspace/.env /home/vagrant/.env
        SHELL      
        # node_config.vm.synced_folder "./shared", "/home/vagrant/shared"
      end
    end
  end
  