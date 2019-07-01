# Trace-Creator

Toolset for automated creation of full packet traces representing annotated units

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
