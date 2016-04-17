# -*- mode: ruby -*-
# # vi: set ft=ruby :

require 'fileutils'

Vagrant.require_version '>= 1.6.0'
Vagrant.configure('2') do |config|
  # always use Vagrants insecure key
  config.ssh.insert_key = false

  config.vm.box = 'coreos-alpha'
  config.vm.box_url = 'https://storage.googleapis.com/alpha.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json'

  config.vm.provider :virtualbox do |v|
    # On VirtualBox, we don't have guest additions or a functional vboxsf
    # in CoreOS, so tell Vagrant that so it can be smarter.
    v.check_guest_additions = false
    v.functional_vboxsf     = false
  end

  # plugin conflict
  if Vagrant.has_plugin?('vagrant-vbguest') then
    config.vbguest.auto_update = false
  end

  config.vm.provider :virtualbox do |vb|
    vb.memory = 2048
    vb.cpus = 1
  end

  config.vm.synced_folder './data', '/var/lib/coreos-vagrant', type: 'rsync'

  config.vm.provision :shell, :inline => 'coreos-cloudinit --from-file /var/lib/coreos-vagrant/user-data', :privileged => true

  config.vm.define 'coreos-01' do |config|
    config.vm.hostname = 'coreos-01'
    config.vm.network :private_network, ip: '172.17.8.101'
    # prometheus
    config.vm.network 'forwarded_port', host: 19090, guest: 9090
    # cAdvisor
    config.vm.network 'forwarded_port', host: 18080, guest: 8080
    # grafana
    config.vm.network 'forwarded_port', host: 13000, guest: 3000
  end
end
