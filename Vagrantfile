# Vagrantfile

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"

  # config.vm.box_check_update = false

  config.vm.network "forwarded_port", guest: 80, host: 10080
  config.vm.network "forwarded_port", guest: 10051, host: 10051

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo -E apt-get update
    sudo -E apt-get install -y vim supervisor
    sudo -E wget -qO- https://get.docker.com/ | sh
    sudo usermod -aG docker vagrant
    curl -L https://github.com/docker/compose/releases/download/1.6.0/docker-compose-`uname -s`-`uname -m` > /tmp/docker-compose
    sudo mv /tmp/docker-compose /usr/local/bin/
    sudo chmod +x /usr/local/bin/docker-compose
    sudo /etc/init.d/docker restart
    sudo mkdir -p /etc/supervisor/conf.d
    mkdir -p /home/ubuntu/zabbix
    cp /vagrant/zabbix/zabbix.yaml /home/ubuntu/zabbix/
    sudo cp /vagrant/zabbix/supervisord_zabbix.conf /etc/supervisor/conf.d/
    sudo service supervisor restart
  SHELL
end
