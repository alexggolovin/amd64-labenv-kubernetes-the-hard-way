# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<-SCRIPT 
      echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCU51oP3300wVIB9mgWzH+x6Wdl1JtaD8Yyq1im6uAWaiHfrjANMXGKb0G6QTYCLNQOXE+7c1B1VQnuwxEau0vhMiOBwGvqJ34Q9XoMYpGctvHpiAkhM2+1rD+/NTu6h+k9KohZ5feQ72UZJGLAzmrPbL9s+CtvjyTvEOXYQcR7skoQpgAuvwgWkFxXuBC4Y26InPFJ4igBKveZqdy7bqXIGbzJQtTB/A0EcUn3pDfFgArxkoD/cQpCKAfTOJoI/j0hmkOdO+q2symvZp3xQ5MTztX/IpF/RKW6rSZ/fNPVpDVas5sWD9Z34g8a2gUaMmU/vkNyAeAL6U1ycWEhHMS3 mrkali@kalilinux" >> /root/.ssh/authorized_keys && echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCU51oP3300wVIB9mgWzH+x6Wdl1JtaD8Yyq1im6uAWaiHfrjANMXGKb0G6QTYCLNQOXE+7c1B1VQnuwxEau0vhMiOBwGvqJ34Q9XoMYpGctvHpiAkhM2+1rD+/NTu6h+k9KohZ5feQ72UZJGLAzmrPbL9s+CtvjyTvEOXYQcR7skoQpgAuvwgWkFxXuBC4Y26InPFJ4igBKveZqdy7bqXIGbzJQtTB/A0EcUn3pDfFgArxkoD/cQpCKAfTOJoI/j0hmkOdO+q2symvZp3xQ5MTztX/IpF/RKW6rSZ/fNPVpDVas5sWD9Z34g8a2gUaMmU/vkNyAeAL6U1ycWEhHMS3 mrkali@kalilinux" >> /home/vagrant/.ssh/authorized_keys && echo "192.168.56.10 server.kubernetes.local server" >> /etc/hosts && echo "192.168.56.11 node-0.kubernetes.local node-0" >> /etc/hosts && echo "192.168.56.12 node-1.kubernetes.local node-1" >> /etc/hosts && echo "192.168.56.13 jumpbox.kubernetes.local jumpbox" >> /etc/hosts && sudo apt update && sudo apt -y install net-tools
SCRIPT

Vagrant.configure("2") do |config|
    config.vm.provision "shell", inline: "echo Provisioning K8s Server"
    config.vm.define "k8s_server" do |k8s_server|
    k8s_server.vm.box = "debian/bookworm64"
    k8s_server.vm.hostname = "server.kubernetes.local"
    k8s_server.vm.network "private_network", ip: "192.168.56.10", hostname: true
    config.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "2048"
  end 
 end
end

Vagrant.configure("2") do |config|
    config.vm.provision "shell", inline: "echo Provisioning K8s Node - 0"
    config.vm.define "k8s_node_0" do |k8s_node_0|
    k8s_node_0.vm.box = "debian/bookworm64"
    k8s_node_0.vm.hostname = "node-0.kubernetes.local"
    k8s_node_0.vm.network "private_network", ip: "192.168.56.11", hostname: true
    config.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "2048"
  end 
 end
end
  
Vagrant.configure("2") do |config|
    config.vm.provision "shell", inline: "echo Provisioning K8s Node - 1"
    config.vm.define "k8s_node_1" do |k8s_node_1|
    k8s_node_1.vm.box = "debian/bookworm64"
    k8s_node_1.vm.hostname = "node-1.kubernetes.local"
    k8s_node_1.vm.network "private_network", ip: "192.168.56.12", hostname: true
    config.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "2048"
  end
 end
end

Vagrant.configure("2") do |config|
    config.vm.provision "shell", inline: "echo Provisioning K8s JumpBox"
    config.vm.define "k8s_jumpbox" do |k8s_jumpbox|
    k8s_jumpbox.vm.box = "debian/bookworm64"
    k8s_jumpbox.vm.hostname = "jumpbox.kubernetes.local"
    k8s_jumpbox.vm.network "private_network", ip: "192.168.56.13", hostname: true
    config.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = "512"
    config.vm.provision "file", source: "id_rsa", destination: "~/.ssh/id_rsa"
    config.vm.provision "file", source: "machines.txt", destination: "~/machines.txt"
    config.vm.provision "shell", run: "always", inline: <<-SHELL 
      sudo cp /home/vagrant/.ssh/id_rsa /root/.ssh/id_rsa
      sudo chmod 600 /root/.ssh/id_rsa
      sudo cp /home/vagrant/machines.txt /root/machines.txt
    SHELL
  end
 end
end

Vagrant.configure("2") do |config|
# run provision script
  config.vm.provision "shell" , inline: $script
end
