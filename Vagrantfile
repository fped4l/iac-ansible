# to be able to configure typed triggers
#ENV['VAGRANT_EXPERIMENTAL'] = 'typed_triggers'

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
  end
  
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["~/.vagrant.d/insecure_private_key"]
  

  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "ubuntu/bionic64"
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
    apt-get install sshpass
    chmod 0600 /home/vagrant/.ssh/insecure_private_key
    ssh-keygen -p -N "" -f /home/vagrant/.ssh/insecure_private_key
    pip install pywinrm
    pip install requests-credssp
    pip install requests-kerberos
    pip install omsdk --upgrade
    ansible-galaxy collection install dellemc.os10
    ansible-galaxy collection install dellemc.openmanage
    ansible-galaxy collection install ansible.windows
    ansible-galaxy collection install chocolatey.chocolatey
    ansible-galaxy collection install community.vmware
    echo "192.168.22.11  ubuntu-01.local ubuntu-01" >> /etc/hosts
    echo "192.168.22.12  win10-01.local win10-01" >> /etc/hosts
    echo "192.168.22.13  win2k19-01.local win2k19-01" >> /etc/hosts
    cd /home/vagrant
    git clone https://github.com/fped4l/iac-ansible.git
    EOF
  end

  config.vm.define "ubuntu" do |ubuntu|
    ubuntu.vm.box = "ubuntu/bionic64"
    ubuntu.vm.hostname = "ubuntu-01"
    ubuntu.vm.network "private_network", ip: "192.168.22.11"
    ubuntu.vm.provision "shell", inline: <<-EOF
    apt-get update
    apt-get -y install python3-pip
    EOF
    
  end

  config.vm.define "win10" do |win10|
    win10.vm.box = "StefanScherer/windows_10"
    win10.vm.hostname = "win10-01"
    win10.vm.network "private_network", ip: "192.168.22.12"
    win10.vm.provision "file", source: "./shared/config-winrm.ps1", destination: "C:/Users/vagrant/config-winrm.ps1"
    win10.vm.provision "shell", inline: <<-EOF
    Set-NetFirewallProfile -Profile * -Enabled False
    Set-ExecutionPolicy -ExecutionPolicy Bypass -Confirm:$false
    & "C:/Users/vagrant/config-winrm.ps1" -verbose
    EOF
  end

  #config.vm.define "win2k19" do |win2k19|
    #win2k19.vm.box = "StefanScherer/windows_2019"
    #win2k19.vm.box_version = "v2021.04.14"
    #win2k19.vm.hostname = "win2k19-01"
    #win2k19.vm.network "private_network", ip: "192.168.22.13"
    #win2k19.vm.provision "file", source: "./shared/config-winrm.ps1", destination: "C:/Users/vagrant/config-winrm.ps1"
    #win2k19.vm.provision "shell", inline: <<-EOF
    #Set-NetFirewallProfile -Profile * -Enabled False
    #Set-ExecutionPolicy -ExecutionPolicy Bypass -Confirm:$false
    #& "C:/Users/vagrant/config-winrm.ps1" -verbose
    #EOF
  #end
end
