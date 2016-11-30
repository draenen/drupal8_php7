# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = '2'
Vagrant.require_version '>= 1.8.6'

# Vagrant configuration.
vconfig = {
  "box" => "geerlingguy/ubuntu1604",
  "user" => "vagrant",
  "hostname" => "drupal8-php7.dev",
  "machine_name" => "drupal8_php7",
  "ip" => "192.168.50.15",
  "memory" => "1024",
  "cpus" => "2"
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Networking configuration.
  config.vm.hostname = vconfig['hostname']
  config.vm.network :private_network, ip: vconfig['ip']

  # SSH options.
  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  # Vagrant box.
  config.vm.box = vconfig['box']

  # Synced folders.
  config.vm.synced_folder "sites", "/var/www/", :nfs => false, :owner => "www-data", :group => "www-data"

  # VirtualBox.
  config.vm.provider :virtualbox do |v|
    v.linked_clone = true if Vagrant::VERSION =~ /^1.8/
    v.name = vconfig['hostname']
    v.memory = vconfig['memory']
    v.cpus = vconfig['cpus']
    v.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    v.customize ['modifyvm', :id, '--ioapic', 'on']
  end

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define vconfig['machine_name']

  # Ansible Provisioning
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
    ansible.galaxy_role_file = "ansible/requirements.yml"
    ansible.extra_vars = {
      config_dir: "/vagrant"
    }
  end
end
