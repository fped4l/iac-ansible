## Getting Started

### Prerequisites

This is an example of how to list things you need to use the software and how to install them.
* Install Vagrant from here: https://www.vagrantup.com/downloads

### Deployment

1. Clone the repo
   ```sh
   git clone https://github.com/fped4l/iac-ansible.git
   ```
2. Deploy development Ansible Environment
   ```sh
   vagrant up; vagrant ssh ansible

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
