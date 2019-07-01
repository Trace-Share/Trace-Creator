# -*- mode: ruby -*-
# vi: set ft=ruby :


# """
# This script serves as an input for Vagrant provisioner to deploy
# Trace-Creator toolset.
#
# All configuration should be set in "./configuration/deployment.yml" file. Please
# don't change the code below unless you know what you're doing.
#
# Provisioning may take a long time, do not stop it or suspend it earlier, it
# can cause unexpected behavior.
#
#
# Usage:
#  * vagrant up              -- deploy and provision the framework
#  * vagrant up <node name>  -- deploy and provision specified node
#  * vagrant halt            -- stop the running framework
#  * vagrant destroy         -- destroy the deployed framework
#  * vagrant -h              -- show common vagrant commands
# """


require 'yaml'  # YAML configuration parser


class TraceCreatorProvisioning
    # """
    # Class for Ansible and bash provisioning of TraceCreator to provision based on
    # configuration file.
    # """

    def initialize(vagrant_config)
        # """
        # Initialize class and store Vagrant config variable.
        #
        # :param vagrant_config: parsed configuration in a dictionary format
        # """
        @vagrant_config = vagrant_config
        @creator_guest = false
    end


    def provision_ansible(vagrant_guest, playbook, requirements)
        # """
        # Add Ansible provisioning configuration to the given guest.
        #
        # Uses the lates Ansible installed via pip.
        #
        # :param vagrant_guest: initialized Vagrant guest
        # :param playbook: Ansible playbook
    # :param requirements: Ansible galaxy role requirements
        # """
        if playbook
            vagrant_guest.vm.provision :ansible_local do |ansible|
                ansible.galaxy_role_file = requirements
                ansible.playbook = playbook
            end
        end
    end


    def provision_bash(vagrant_guest, script)
        # """
        # Add BASH provisioning configuration to the given guest.
        #
        # :param vagrant_guest: initialized Vagrant guest
        # :param script: bash script to run
        # """
        if script
            vagrant_guest.vm.provision :shell, path: script
        end
    end


    def start_creator(vagrant_guest)
        # """
        # Start trace-creator script to run defined commands and store traffic capture.
        #
        # :param vagrant_guest: initialized Vagrant guest
        # """
        vagrant_guest.vm.provision :shell do |s|
            # PYTHONUNBUFFERED: make vagrant display real-time output of the shell
            s.inline = "export PYTHONUNBUFFERED=1; python /vagrant/trace-creator.py"
        end
    end


    def deploy_guest(guest_config)
        # """
        # Set guest deployment and provisioning based on given configuration.
        #
        # :param guest_config: parsed configuration of one guest
        # """
        if guest_config["creator"] and !@creator_guest
            # Store guest configuration to deploy it as last
            @creator_guest = guest_config
        else
            @vagrant_config.vm.define guest_config["name"] do |guest|
                guest.vm.hostname = guest_config["name"]
                guest.vm.box = guest_config["box"]
                guest.vm.box_url = guest_config["box_url"]
                guest.vm.network :private_network, ip: guest_config["ip"],
                    netmask: guest_config["mask"]
                    
                guest.vm.provider :virtualbox do |v|
                    v.customize ["modifyvm", :id, "--name", guest_config["name"]]
                    v.customize ["modifyvm", :id, "--cpus", guest_config["cpu"]]
                    v.customize ["modifyvm", :id, "--memory", guest_config["memory"]]
                    v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
                    v.customize ["modifyvm", :id, "--uartmode1", "disconnected"]
                end
                
                provision_ansible(guest, guest_config["ansible"], guest_config["ansible_requirements"])
                provision_bash(guest, guest_config["bash"])  
                
                # Check if start trace-creator script 
                if guest_config["creator"]
                    start_creator(guest)
                end
            end
        end
    end


    def deploy_creator()
        # """
        # Start deployment of the guest selected as a creator.
        #
        # """
        if @creator_guest
            deploy_guest(@creator_guest)
        end
    end
end


# """
# Vagrant main file. All Vagrant configuration is done below.
# """
Vagrant.configure("2") do |config|
    provision_configuration = YAML.load_file("configuration/deployment.yml")
    trace_creator_provisioning = TraceCreatorProvisioning.new(config)

    provision_configuration.each do |guest_configuration|
        trace_creator_provisioning.deploy_guest(guest_configuration)
    end

    trace_creator_provisioning.deploy_creator()
end
