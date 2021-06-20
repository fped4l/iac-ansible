# to be able to configure typed triggers
#ENV['VAGRANT_EXPERIMENTAL'] = 'typed_triggers'

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/groovy64"
  #config.vm.synced_folder "./", "/home/vagrant", create: true
  config.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 2
  end
  
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["keys/vagrant_rsa", "~/.vagrant.d/insecure_private_key"]
  config.vm.provision "file", source: "keys/vagrant_rsa.pub", destination: "~/.ssh/authorized_keys"
  config.vm.provision "shell", inline: <<-EOF
    apt-get update
    apt-get -y install git
    apt-get -y install python3-pip
    EOF
  
  
  config.vm.define "ansible" do |ansible|
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.22.10"
    ansible.vm.provision "shell", inline: <<-EOF
    apt-get -y install ansible
    pip install omsdk --upgrade
    ansible-galaxy collection install dellemc.os10
    ansible-galaxy collection install dellemc.openmanage
    ansible-galaxy collection install ansible.windows
    ansible-galaxy collection install community.vmware
    EOF
  end

  config.vm.define "test1" do |test1|
    test1.vm.hostname = "test1"
    test1.vm.network "private_network", ip: "192.168.22.11"
  end
end