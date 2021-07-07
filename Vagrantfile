# to be able to configure typed triggers
#ENV['VAGRANT_EXPERIMENTAL'] = 'typed_triggers'

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
  end
  
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["keys/vagrant_rsa", "~/.vagrant.d/insecure_private_key"]
  #config.vm.provision "file", source: "keys/vagrant_rsa.pub", destination: "~/.ssh/vagrant_rsa.pub"
  

  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "ubuntu/groovy64"
    ansible.vm.hostname = "ansible"
    #ansible.vm.box_url = "ubuntu/groovy64"
    ansible.vm.network "private_network", ip: "192.168.22.10"
    ansible.vm.provision "file", source: "keys/vagrant_rsa", destination: "~/.ssh/vagrant_rsa"
    ansible.vm.provision "file", source: "keys/vagrant_rsa", destination: "home/vagrant/.ssh/vagrant_rsa"
    ansible.vm.provision "shell", inline: <<-EOF
    apt-get update
    apt-get -y install git
    apt-get -y install python3-pip
    apt-get -y install ansible
    pip install omsdk --upgrade
    ansible-galaxy collection install dellemc.os10
    ansible-galaxy collection install dellemc.openmanage
    ansible-galaxy collection install ansible.windows
    ansible-galaxy collection install community.vmware
    cd /home/vagrant
    git clone https://github.com/fped4l/iac-ansible.git
    EOF
  end

  config.vm.define "ubuntu" do |ubuntu|
    ubuntu.vm.box = "ubuntu/groovy64"
    ubuntu.vm.hostname = "ubuntu"
    #ubuntu.vm.box_url = "ubuntu/groovy64"
    ubuntu.vm.network "private_network", ip: "192.168.22.11"
    ubuntu.vm.provision "file", source: "keys/vagrant_rsa.pub", destination: "~/.ssh/vagrant_rsa.pub"
    ubuntu.vm.provision "file", source: "keys/vagrant_rsa.pub", destination: "home/vagrant/.ssh/vagrant_rsa.pub"
    ubuntu.vm.provision "shell", inline: <<-EOF
    apt-get update
    cat ~/.ssh/vagrant_rsa.pub >> ~/.ssh/authorized_keys
    cat ~/.ssh/vagrant_rsa.pub >> /home/vagrant/.ssh/authorized_keys
    EOF
    
  end

  config.vm.define "windows" do |windows|
    windows.vm.box = "StefanScherer/windows_10"
    windows.vm.hostname = "windows"
    #windows.vm.box_url = "StefanScherer/windows_10"
    windows.vm.network "private_network", ip: "192.168.22.12"
    windows.vm.provision "file", source: "keys/vagrant_rsa.pub", destination: "C:/Users/vagrant/.ssh/vagrant_rsa.pub"
    windows.vm.provision "shell", inline: <<-EOF
    Set-NetFirewallProfile -Profile * -Enabled False
    EOF
  end
end