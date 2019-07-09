# Trace-Share: Trace-Creator

Toolset for automated creation of network traffic traces

### Table of Contents

* [Requirements](#requirements)
* [Getting started](#getting-started)
   + [Environment Configuration](#environment-configuration)
   + [Commands Configuration](#commands-configuration)
   + [Toolset Control](#toolset-control)
* [Contribution](#contribution)


## Requirements

Trace-Creator toolset is provided as an automated virtual environment able to run given commands and capture corresponding network traffic. The virtual environment is operated by [**Vagrant**](https://www.vagrantup.com/) tool with [**VirtualBox**](https://www.virtualbox.org/) virtualization. Any other software requirements are automatically handled by the toolset directly within virtual machines.

We are trying to stay up to date with the latest versions of all used software and provisioning tools. Be sure that you are using the latest versions of Vagrant and VirtualBox to run the Trace-Creator toolset correctly.


## Getting Started

Trace-Creator emulates interactions between one source and multiple destination devices using a virtual environment automatically operated by Vagrant tool. Once the virtual environment is set up, a generating script ([trace-creator.py](./trace-creator.py)) automatically starts to execute specified commands and capture corresponding network traffic. Resulting network traffic traces are available in the `./capture/` directory after the execution of all specified commands.

### Environment Configuration

Trace-Creator allows you to set up one source and multiple destination guests. Environment specification and guest settings are specified in `./configuration/deployment.yml` configuration file providing the following mandatory configuration options:

* **name**: The name of the guest machine
```yaml
name: source
```
* **box** and **box_url**: Virtual machine box (see [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search))
```yaml
box: ubuntu/bionic64
box_url: ubuntu/bionic64
```
* **ip**: Address of the guest in the virtual private network
```yaml
ip: 10.0.0.2
```
* **mask**: Address mask of the virtual private network
```yaml
mask: 255.255.255.0
```
* **cpu**: Number of virtual CPUs
```yaml
cpu: 1
```
* **memory**: The size of main memory is in MB
```yaml
memory: 1024
```
* **ansible_requirements**: Ansible requirements file (external roles include)
```yaml
ansible_requirements: configuration/provision/ansible/requirements.yml
```
* **ansible**: Ansible playbook provision file
```yaml
ansible: configuration/provision/ansible/source.yml
```
* **bash**: Bash provision file
```yaml
bash: configuration/provision/bash/source-1.sh
```
* **creator**: Execute trace-creator generating script on this guest (only one guest can be set to "true")
```yaml
creator: true
```

The configuration file allows defining any number of guests that can be provisioned using both Bash and [Ansible](https://www.ansible.com/). If the Bash provisioning is used, all commands in the specified file are directly executed in the guest machine. In addition to simple Bash provisioning, Ansible provisioning can be used, providing more advanced options (see official documentation at [https://docs.ansible.com/ansible/latest/](https://docs.ansible.com/ansible/latest/)). If both provisioning options are used, the Ansible is started before Bash.

Example of environment configuration is available in [**./configuration/deployment.yml**](./configuration/deployment.yml) file specifying two virtual machines with various provisioning settings.

### Commands Configuration

Once the environment is set, the Trace-Creator toolset runs a generating script ([trace-creator.py](./trace-creator.py)) that starts to execute commands and capture corresponding network traffic based on settings in `./configuration/trace-creator.yml` configuration file.  The configuration file provides the following options:

* **name** (mandatory): The name of the command (will be part of output files name)
```yaml
name: Nmap without ICMP
```
* **command** (mandatory): Executed command (simple command or script)
```yaml
command: nmap -sW 10.0.0.3 -v
```
* **filter**: Filter of network traffic capture (see [https://wiki.wireshark.org/CaptureFilters](https://wiki.wireshark.org/CaptureFilters))
```yaml
filter:  not icmp
```
* **configuration**: Commands performed before the main command is executed for additional settings of the virtual environment
  * **ip** – address of the host that will be configured
  * **command** – configuration command executed on the given host (use [netem](https://wiki.linuxfoundation.org/networking/netem) tool to change network properties)
```yaml
configuration:
  - ip: 10.0.0.3
    command: bash /vagrant/configuration/trace-creator/destination-1.sh
```

Example of commands configuration is available in [**./configuration/trace-creator.yml**](./configuration/trace-creator.yml) file containing two Nmap scan commands.

### Toolset Control

The Trace-Share toolset is controlled simply via [Vagrant command-line interface](https://www.vagrantup.com/docs/cli/). Use the following commands when both environment and commands are configured:

- `$ vagrant up` – automatically create and provision virtual machines and start the generating script
- `$ vagrant provision <guest_name>` – rerun provisioning if an error occurred
- `$ vagrant destroy` – destroy the virtual environment after all network traffic traces are obtained


## Contribution

Pull requests are welcome! For major changes, please open an issue first to discuss what you would like to change.

*If you are interested in research collaborations, don't hesitate to contact us at  [https://csirt.muni.cz](https://csirt.muni.cz/about-us/contact?lang=en)!*
