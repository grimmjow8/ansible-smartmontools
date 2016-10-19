# -*- mode: ruby -*-
# vi: set ft=ruby :

boxes = [
  {
    :name => "LinuxMint17",
    :os   => "npalm/mint17-amd64-cinnamon",
    :eth1 => "192.168.88.21",
    :mem  => "512",
    :cpu  => "1"
  },
  {
    :name => "Ubuntu14",
    :os   => "ubuntu/trusty64",
    :eth1 => "192.168.88.22",
    :mem  => "512",
    :cpu  => "1"
  },
  {
    :name => "Debian8",
    :os   => "debian/jessie64",
    :eth1 => "192.168.88.23",
    :mem  => "512",
    :cpu  => "1"
  }
]

Vagrant.configure(2) do |config|

  # Turn off shared folders
  config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

  boxes.each do |opts|
    config.vm.box = opts[:os]
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]

      # set memory for vms
      config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", opts[:mem]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
      end

      # set network ips
      config.vm.network :private_network, ip: opts[:eth1]

      # deploy ansible files
      config.vm.provision "file", source: "files", destination: "~/files"

      # provision ansible
      config.vm.provision 'ansible' do |ansible|
        ansible.playbook = 'tests/test.yml'
        ansible.sudo = true
#        ansible.verbose = 'vvvvvv'
        ansible.host_key_checking = false
      end
    end
  end
end