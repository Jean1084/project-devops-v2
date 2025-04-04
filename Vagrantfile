# Définition des scripts de setup
SETUP_PROJECT_SCRIPT = "setup/setup_project.sh"
SETUP_MASTER = "setup/setup_master.sh"
SETUP_WORDPRESS = "setup/setup_wordpress.sh"
SETUP_MYSQL = "setup/setup_mysql.sh"
SETUP_REGISTRY = "setup/setup_registry.sh"

Vagrant.configure("2") do |config|
  # Définition de l'image de base
  config.vm.box = "ubuntu/focal64"

  # Liste des nœuds avec leur IP et script spécifique
  nodes = [
    { name: "master", ip: "192.168.56.10", script: SETUP_MASTER },
    { name: "wordpress", ip: "192.168.56.11", script: SETUP_WORDPRESS },
    { name: "mysql", ip: "192.168.56.12", script: SETUP_MYSQL },
    { name: "registry", ip: "192.168.56.13", script: SETUP_REGISTRY }
  ]

  # Création des machines et configuration de base
  nodes.each do |node|
    config.vm.define node[:name] do |node_config|
      node_config.vm.hostname = node[:name]
      node_config.vm.network "private_network", ip: node[:ip]

      node_config.vm.provider "virtualbox" do |vb|
        vb.memory = "1024"
        vb.cpus = 1
        vb.name = "vm-#{node[:name]}"
      end

      # Synchronisation des fichiers
      node_config.vm.synced_folder ".", "/home/vagrant/workspace", type: "virtualbox"
      node_config.vm.provision "shell", inline: <<-SHELL
        ln -sf /home/vagrant/workspace/.env /home/vagrant/.env
      SHELL
    end
  end

  # Provisioning spécifique pour le master : clé SSH + copie vers autres nœuds
  config.vm.define "master" do |master_config|
    master_config.vm.provision "shell", inline: <<-SHELL
      echo "Génération de la clé SSH sur le Master..."
      sudo -u vagrant ssh-keygen -t rsa -b 4096 -f /home/vagrant/.ssh/id_rsa -N ""

      echo "Mise à jour et installation de sshpass..."
      sudo apt update && sudo apt install -y sshpass
    SHELL
  end

  # Exécution du script setup_project.sh et des scripts spécifiques sur chaque VM
  nodes.each do |node|
    config.vm.define node[:name] do |node_config|
      node_config.vm.provision :shell, path: SETUP_PROJECT_SCRIPT
      node_config.vm.provision "shell", inline: <<-SHELL
        echo "Fin du script setup_project pour #{node[:name]}..."
        sleep 60
      SHELL

      # Ajout de l'exécution du script spécifique à chaque VM
      node_config.vm.provision :shell, path: node[:script]
    end
  end
end
