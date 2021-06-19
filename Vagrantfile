# to be able to configure typed triggers
#ENV['VAGRANT_EXPERIMENTAL'] = 'typed_triggers'

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/groovy64"
  #config.vm.synced_folder "./.vagrant/shared", "/home/vagrant", create: true
  config.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 2
  end
  
  config.vm.provision "shell", inline: <<-EOF
    apt-get update
    apt-get -y install git
    apt-get -y install ansible
    ansible-galaxy collection install dellemc.os10
    ansible-galaxy collection install ansible.windows
    ansible-galaxy collection install community.vmware
    EOF
  
  config.vm.define "ansible" do |ansible|
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.22.10"
    #ansible.vm.provision "shell", inline: <<-EOF
    # export local_ip="$(ip route | grep eth1 | cut -d' ' -f9)"  
    #  microk8s.add-node -l 500 -t b1e90349c50c4d2b657e4d4cf3d10b74
    #EOF
  end 

end