# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

dev_vagrant = YAML.load_file 'ansible/env_vars/development.yml'
base_vagrant = YAML.load_file 'ansible/env_vars/base.yml'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu-14.04"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  config.vm.network :private_network, ip: dev_vagrant['instance_ipaddr']
  config.ssh.forward_x11 = true
  config.vm.synced_folder ".", "/vagrant", type: "nfs", mount_options: %w{nolock,vers=3,udp,noatime,actimeo=1}
  #config.ssh.username = base_vagrant['django_project_name']

  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 2
  end

  # Ansible provisioner.
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/dev-vagrant.yml"
    ansible.host_key_checking = false
    ansible.verbose = "v"
  end
end
