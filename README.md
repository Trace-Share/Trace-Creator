# Trace-Share: Trace-Creator

Toolset for automated creation of network traffic traces


## Requirements

Trace-Creator toolset is provided as an automated virtual environment able to run given commands and capture corresponding network traffic. The virtual environment is operated by [**Vagrant**](https://www.vagrantup.com/) tool with [**VirtualBox**](https://www.virtualbox.org/) virtualization. Any other software requirements are automatically handled by the toolset directly within virtual machines.

We are trying to stay up to date with the latest versions of all used software and provisioning tools. Be sure that you are using the latest versions of Vagrant and VirtualBox to run the Trace-Creator toolset correctly.


## Getting started

We have it all prepared for you. The basic setup is preconfigured. You have to only set following configuration files 
to prepare the environment:

- [configuration/deployment.yml](configuration/deployment.yml) – set system properties of virtual guests and specify 
additional provisioning files in Ansible od Bash
- [configuration/trace-creator.yml](configuration/trace-creator.yml) – specify commands that should be performed on guests


### Basic commands

- `$ vagrant up` – create and configure guest machines according to configuration files and run specified commands (all 
capture files together with logs will be available in ./capture/ directory)
- `$ vagrant provision attacker` – rerun creation script if an error occurred
- `$ vagrant destroy` – destroy the virtual environment after the collection


## Contribution

Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.

*If you are interested in research collaborations, don't hesitate to contact us at  [https://csirt.muni.cz](https://csirt.muni.cz/about-us/contact?lang=en)!*
