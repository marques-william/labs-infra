# Vagrantfile - labs de estudo
# Autor: William Ferreira Marques
# Criado em: 21/11/2026
# Projeto pessoal de estudo (ferias 21/11 a 03/12/2026)
#
# 3 VMs Rocky Linux 9 numa rede privada 192.168.56.0/24
# Comandos: vagrant up | vagrant ssh node1 | vagrant halt | vagrant destroy -f

VMS = {
  "node1" => { ip: "192.168.56.11", mem: 2048 },
  "node2" => { ip: "192.168.56.12", mem: 2048 },
  "node3" => { ip: "192.168.56.13", mem: 2048 },
}

Vagrant.configure("2") do |config|
  config.vm.box = "bento/rockylinux-9"

  VMS.each do |nome, cfg|
    config.vm.define nome do |vm|
      vm.vm.hostname = nome
      vm.vm.network "private_network", ip: cfg[:ip]

      vm.vm.provider "virtualbox" do |vb|
        vb.name = "labs-#{nome}"
        vb.memory = cfg[:mem]
        vb.cpus = 2
      end

      # cada VM conhece as outras pelo nome
      vm.vm.provision "shell", inline: <<-SHELL
        grep -q "node1" /etc/hosts || echo "192.168.56.11 node1" >> /etc/hosts
        grep -q "node2" /etc/hosts || echo "192.168.56.12 node2" >> /etc/hosts
        grep -q "node3" /etc/hosts || echo "192.168.56.13 node3" >> /etc/hosts
      SHELL
    end
  end
end
