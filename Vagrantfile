# set up the default terminal
ENV["TERM"]="linux"

Vagrant.configure("2") do |config|

  # set up root access 
  config.ssh.username = 'root'
  config.ssh.password = 'vagrant'
  config.ssh.insert_key = 'true'

  NodeCount = 2

  # Kubernetes Nodes
  (1..NodeCount).each do |i|
    config.vm.define "node#{i}" do |node|
      # set the image for the vagrant box
      config.vm.box = "opensuse/Leap-15.2.x86_64"

      node.vm.hostname = "node#{i}.example.com"
      # set the static IP for the vagrant box
      node.vm.network "private_network", ip: "192.168.50.10#{i}"
      # configure the parameters for VirtualBox provider
      node.vm.provider "virtualbox" do |v|
        v.name = "node#{i}"
        v.memory = 2048
        v.cpus = 2
      end

    config.vm.provision "shell", inline: <<-SHELL
      # install Docker
      zypper --non-interactive install docker python3-docker-compose
      systemctl enable docker
      usermod -G docker -a $USER
      systemctl restart docker
    SHELL
    #config.vm.provision "shell", path: "bootstrap.sh"
    end
  end
end