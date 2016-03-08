#!/usr/bin/env ruby

# method to insert a ssh key into the vm
require_relative './data/ansible/utils/key_authorization'

# prebuilt image type
# more machines are available at https://atlas.hashicorp.com/boxes/search
ubuntu_box = 'ubuntu/trusty64'
open_suse_box = 'webhippie/opensuse-13.2'

box_type = ubuntu_box

if (ENV['box_type'].to_s == 'opensuse')
  box_type = open_suse_box
end


ip_address = '192.168.34.11'
rsa_key = '~/.ssh/vagrant_rsa.pub'
host_name = 'git2prov.dev'

# https://docs.vagrantup.com/v2/providers/index.html
# possible values: docker, vmware, hyper-v
# additional plugins are available
vm_provider = 'virtualbox'

# ports that should be forwarded to the host
ports = [
  { 'guest' => 8905, 'host' => 8905 }
]

Vagrant.configure(2) do | config |

  # inserts a key into the machine so we can work on it
  # emulates a remote machine workflow
  # otherwise using vagrant ssh also works
  authorize_key_for_root config, rsa_key

  # disable librarian chef plugin
  if Vagrant.has_plugin?("vagrant-librarian-chef")
    config.librarian_chef.enabled = false
  end

  if Vagrant.has_plugin?("vagrant-hostsupdater")
    config.hostsupdater.remove_on_suspend = false
  end

  # configure vm values
  config.vm.hostname = host_name
  config.vm.box = box_type
  config.vm.network 'private_network', ip: ip_address

  # forward given ports: see ports array
  ports.each do | port |
    config.vm.network 'forwarded_port', guest: port['guest'], host: port['host']
  end

  # customize vm provider
  config.vm.provider vm_provider do | vb |
    vb.memory = 2048
    vb.cpus = 2
  end

end
