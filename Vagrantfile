# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "byo-atomic"

  config.vm.box = "yungsang/boot2docker"

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end

  config.vm.network :forwarded_port, guest: 2375, host: 2375, disabled: true

  config.vm.network :private_network, ip: "192.168.33.201"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provision :docker do |d|
    d.build_image "/vagrant/", args: "--rm -t yungsang/byo-atomic"
    d.run "byo-atomic", image: "yungsang/byo-atomic", args: "-p 10080:80"
  end

  config.vm.provision :shell do |sh|
    sh.inline = <<-EOT
      docker exec byo-atomic rpm-ostree compose tree --repo=/srv/rpm-ostree/repo /fedora-atomic/fedora-atomic-docker-host.json
    EOT
  end
end
