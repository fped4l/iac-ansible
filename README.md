## Getting Started

# Prerequisites

This is the list of prerequisite software you need to deploy the sandbox ethical hacking environment.

**Git**
* Download and install Git from here: https://github.com/git-for-windows/git/releases/download/v2.32.0.windows.2/Git-2.32.0.2-64-bit.exe

**Vagrant**
* Download and install Vagrant from here: https://releases.hashicorp.com/vagrant/2.2.17/vagrant_2.2.17_x86_64.msi

**Oracle VirtualBox**
* Download and install VirtaulBox from here: https://download.virtualbox.org/virtualbox/6.1.22/VirtualBox-6.1.22-144080-Win.exe

# Deployment of Virtual Environment

1. Open Powershell; Clone the repo.
   ```sh
   git clone https://github.com/fped4l/iac-ansible.git
   ```
   ![image](https://user-images.githubusercontent.com/25991921/127525947-fa1308e4-6325-46fe-b225-cf34dc05f361.png)

2. Change to repository directory.
   ```sh
   cd .\iac-ansible\
   ```
   ![image](https://user-images.githubusercontent.com/25991921/127526172-76f1b0e0-b8c2-4dcb-a51a-df2cf4ef253f.png)

3. Deploy Ansible Development Environment (Will take some time depending on host machine resources)
   ```sh
   vagrant up; vagrant ssh ansible
   ```
   ![image](https://user-images.githubusercontent.com/25991921/127527200-5a014d0f-2678-44de-b9e4-8854c9ef7f56.png)


## Useful commands

* Use Ansible win_ping module to ping the windows group of machines (specified int ~/iac-ansible/inventory.yml)
   ```sh
   ansible windows -u vagrant --ask-pass -m win_ping
   ```

* Use Ansible ping module to ping the linux group of machines (specified int ~/iac-ansible/inventory.yml)  
   ```sh
   ansible linux -m ping
   ```

* Run playbook to install chocolatey
   ```sh
   ansible-playbook -u vagrant --ask-pass ./playbooks/windows/install-chocolatey.yml
   ```

* Run playbook to install VLC
   ```sh
   ansible-playbook -u vagrant --ask-pass ./playbooks/windows/install-vlc-v3011.yml
   ```
