# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  pe_version                    = '2019.0.0'
  config.pe_build.version       = pe_version

######################
## Puppet Master VM ##
######################
  # Define the Master VM Characteristics
  config.vm.define 'master' do |master|
    master.vm.box = 'puppetlabs/centos-7.2-64-nocm'
    master.vm.network :private_network, :ip => '10.10.100.100'
    master.vm.network "forwarded_port", guest: 443, host: 8443
    master.vm.hostname = 'master.puppetlabs.vm'

  # Configure Master VM Settings
  master.vm.provider :virtualbox do |settings|
    settings.memory = 4608
    settings.name = "master_2019.0.0"
    settings.cpus = 2
  end

  # Add all other hosts for environment
  master.vm.provision :hosts do |entries|
    entries.add_host '10.10.100.100', ['master.puppetlabs.vm', 'master']
    entries.add_host '10.10.100.110', ['centos.puppetlabs.vm', 'centos']
    entries.add_host '10.10.100.111', ['ubuntu.puppetlabs.vm', 'ubuntu']
  end

  # Set the PE Role for this node
  master.vm.provision :pe_bootstrap do |provisioner|
    provisioner.role = :master
    provisioner.answer_file = 'provision/pe.conf'
  end
    master.vm.provision :shell, path: "provision/master.sh"
  end

###############
## CentOS VM ##
###############
  # Define the Development VM Characteristics
  config.vm.define 'centos' do |centos|
    centos.vm.box = 'puppetlabs/centos-7.2-64-nocm'
    centos.vm.network :private_network, :ip => '10.10.100.110'
    centos.vm.hostname = 'centos.puppetlabs.vm'

  # Configure CentOS VM Settings
  centos.vm.provider :virtualbox do |settings|
    settings.memory = 1024
    settings.name = "centos_2019.0.0"
    settings.cpus = 1
  end

  # Add all other hosts for environment
  centos.vm.provision :hosts do |entries|
    entries.add_host '10.10.100.100', ['master.puppetlabs.vm', 'master']
    entries.add_host '10.10.100.110', ['centos.puppetlabs.vm', 'centos']
    entries.add_host '10.10.100.111', ['ubuntu.puppetlabs.vm', 'ubuntu']
  end

  # Set the PE Role of This Node
  centos.vm.provision :pe_agent do |provisioner|
    provisioner.master = 'master.puppetlabs.vm'
  end
    centos.vm.provision :shell, path: "provision/centos.sh"
  end

###############
## Ubuntu VM ##
###############
  # Define the Ubuntu VM Characteristics
  config.vm.define 'ubuntu' do |ubuntu|
    ubuntu.vm.box = 'puppetlabs/ubuntu-16.04-64-nocm'
    ubuntu.vm.network :private_network, :ip => '10.10.100.111'
    ubuntu.vm.hostname = 'ubuntu.puppetlabs.vm'

  # Configure Ubuntu VM Settings
  ubuntu.vm.provider :virtualbox do |settings|
    settings.memory = 1024
    settings.name = "ubuntu_2019.0.0"
    settings.cpus = 1
  end

  # Add all other hosts for environment
  ubuntu.vm.provision :hosts do |entries|
    entries.add_host '10.10.100.100', ['master.puppetlabs.vm', 'master']
    entries.add_host '10.10.100.110', ['centos.puppetlabs.vm', 'centos']
    entries.add_host '10.10.100.111', ['ubuntu.puppetlabs.vm', 'ubuntu']
  end

  # Set the PE Role of This Node
  ubuntu.vm.provision :pe_agent do |provisioner|
    provisioner.master = 'master.puppetlabs.vm'
  end
    ubuntu.vm.provision :shell, path: "provision/ubuntu.sh"
  end
end