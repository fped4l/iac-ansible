# to be able to configure typed triggers
#ENV['VAGRANT_EXPERIMENTAL'] = 'typed_triggers'

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
  end
  
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["./keys/vagrant_rsa", "~/.vagrant.d/insecure_private_key"]
  #config.vm.provision "file", source: "./keys/vagrant_rsa.pub", destination: "~/.ssh/vagrant_rsa.pub"
  

  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "ubuntu/groovy64"
    ansible.vm.hostname = "ansible"
    #ansible.vm.box_url = "ubuntu/groovy64"

    ansible.vm.network "private_network", ip: "192.168.22.10"
    ansible.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "~/.ssh/insecure_private_key"
    ansible.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/insecure_private_key"
    ansible.vm.provision "shell", inline: <<-EOF
    apt-get update
    apt-get -y install git
    apt-get -y install python3-pip
    apt-get -y install ansible
    chmod 0600 /home/vagrant/.ssh/insecure_private_key
    ssh-keygen -p -N "" -f /home/vagrant/.ssh/insecure_private_key
    pip install pywinrm
    pip install omsdk --upgrade
    ansible-galaxy collection install dellemc.os10
    ansible-galaxy collection install dellemc.openmanage
    ansible-galaxy collection install ansible.windows
    ansible-galaxy collection install community.vmware
    echo "192.168.22.11  linux.local linux" >> /etc/hosts
    echo "192.168.22.12  windows.local windows" >> /etc/hosts
    cd /home/vagrant
    git clone https://github.com/fped4l/iac-ansible.git
    EOF
  end

  config.vm.define "ubuntu" do |ubuntu|
    ubuntu.vm.box = "ubuntu/groovy64"
    ubuntu.vm.hostname = "ubuntu"
    ubuntu.vm.network "private_network", ip: "192.168.22.11"
    ubuntu.vm.provision "shell", inline: <<-EOF
    apt-get update
    apt-get -y install python3-pip
    EOF
    
  end

  config.vm.define "windows" do |windows|
    windows.vm.box = "StefanScherer/windows_10"
    windows.vm.hostname = "windows"
    windows.vm.network "private_network", ip: "192.168.22.12"
    #config.vm.network "forwarded_port", guest: 5985, host: 8080
    windows.vm.provision "file", source: "./keys/vagrant_rsa.pub", destination: "C:/Users/vagrant/.ssh/vagrant_rsa.pub"
    windows.vm.provision "file", source: "./shared/config-winrm.ps1", destination: "C:/Users/vagrant/config-winrm.ps1"
    windows.vm.provision "shell", inline: <<-EOF
    Set-NetFirewallProfile -Profile * -Enabled False
    Set-ExecutionPolicy -ExecutionPolicy Bypass -Confirm:$false
    & "C:/Users/vagrant/config-winrm.ps1" 
    EOF
  end
end
