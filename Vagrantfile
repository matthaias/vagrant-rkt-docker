# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL
    wget --quiet $(curl -s https://api.github.com/repos/coreos/rkt/releases/latest | grep 'browser_download_url' | grep '.tar.gz' | grep -v '.tar.gz.sig' | awk '{print $2}' | tr -d '"')
    tar xfv $(curl -s https://api.github.com/repos/coreos/rkt/releases/latest | grep "name" | grep ".tar.gz" | grep -v ".tar.gz.sig" | awk '{print $2}' | tr -d '"' | tr -d ,)
    sudo echo "alias rkt='sudo /home/vagrant/rkt-$(curl -s https://api.github.com/repos/coreos/rkt/releases/latest | grep \"tag_name\" | awk '{print $2}' | tr -d '\",')/rkt'" >> /home/vagrant/.bashrc
    sudo chown vagrant:vagrant /etc/apt/sources.list.d/docker.list
    sudo apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
    sudo touch /etc/apt/sources.list.d/docker.list
    sudo chown root:vagrant /etc/apt/sources.list.d/docker.list
    sudo echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" > /etc/apt/sources.list.d/docker.list
    sudo apt-get update
    sudo apt-get install -y docker-engine --force-yes
    sudo gpasswd -a vagrant docker
  SHELL
end
