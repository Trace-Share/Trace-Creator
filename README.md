# Trace-Share: Trace-Creator

Toolset for automated creation of network traffic traces


## Requirements

Trace-Creator toolset is provided as an automated virtual environment able to run given commands and capture corresponding network traffic. The virtual environment is operated by [**Vagrant**](https://www.vagrantup.com/) tool with [**VirtualBox**](https://www.virtualbox.org/) virtualization. Any other software requirements are automatically handled by the toolset directly within virtual machines.

We are trying to stay up to date with the latest versions of all used software and provisioning tools. Be sure that you are using the latest versions of Vagrant and VirtualBox to run the Trace-Creator toolset correctly.


## Getting Started

Trace-Creator emulates interactions between one source and multiple destination devices using a virtual environment automatically operated by Vagrant tool. Once the virtual environment is set up, a generating script ([trace-creator.py](./trace-creator.py)) automatically starts to execute specified commands and capture corresponding network traffic. Resulting network traffic traces are available in the `./capture/` directory after the execution of all specified commands.

### Environment Configuration

Trace-Creator allows you to set up one source and multiple destination guests. Environment specification and guest settings are specified in `./configuration/deployment.yml` configuration file providing the following configuration options:

* `name` – the name of the guest machine
* `box`,`box_url` – virtual machine box (see [https://atlas.hashicorp.com/search](https://atlas.hashicorp.com/search))
* `ip` – address of the guest in the virtual private network
* `mask` – address mask of the virtual private network
* `cpu` – number of virtual CPUs
* `memory` – the size of main memory is in MB
* `ansible_requirements` – Ansible requirements file (external roles include)
* `ansible` – Ansible playbook provision file
* `bash` – Bash provision file
* `creator` – execute trace-creator generating script on this guest (only one guest can be set to "true")

The configuration file allows defining any number of guests that can be provisioned using both Bash and [Ansible](https://www.ansible.com/). If the Bash provisioning is used, all commands in the specified file are directly executed in the guest machine. In addition to simple Bash provisioning, Ansible provisioning can be used, providing more advanced options (see official documentation at [https://docs.ansible.com/ansible/latest/](https://docs.ansible.com/ansible/latest/)). If both provisioning options are used, the Ansible is started before Bash.

Example of environment configuration is available in [**./configuration/deployment.yml**](./configuration/deployment.yml) file specifying two virtual machines with various provisioning options.


### Basic commands

- `$ vagrant up` – create and configure guest machines according to configuration files and run specified commands (all 
capture files together with logs will be available in ./capture/ directory)
- `$ vagrant provision attacker` – rerun creation script if an error occurred
- `$ vagrant destroy` – destroy the virtual environment after the collection


## Contribution

Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.

*If you are interested in research collaborations, don't hesitate to contact us at  [https://csirt.muni.cz](https://csirt.muni.cz/about-us/contact?lang=en)!*
