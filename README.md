# Apigee OPDK Ansible Setup Playbook
The purpose of this project is to help configure Ansible for the use of
[Apigee OPDK roles](https://github.com/carlosfrias/apigee-opdk-playbook-samples).
The Apigee OPDK roles follow conventions in the naming of files and where those files are 
located. This project sets up those locations and performs the git clone to setup your 
environment with the templates that should be customized for your use.

# Quick Installation
Please note that this installation framework uses Ansible. Please install Ansible prior to cloning
this project. Once Ansible is ready for use you can follow these steps: 
1. Clone this project into a workspace. We recommend ~/apigee-workspace.
1. Change into the cloned directory.  
1. Review [repository-mapping-name-folder.yml](repository-mapping-name-folder.yml) to select the
projects, roles and configurations you wish to work with.
1. Update [repository-custom-properties.yml](repository-custom-properties.yml) with the locations 
where you want resources and configuration files located. Defaults are provided. 
1. Execute `ansible-playbook setup.yml` to create a local development environment. 


# What does the Quick Installation do? 
The quick installation will 
1. Update the credentials.yml file as indicated below.
1. Add the license.txt file to `~/.apigee-secure/license.txt`
1. Configure ansible to work with the correct configuration using:
    `export ANSIBLE_CONFIG=~/.ansible/configurations/edge-1701-dc1.cfg`
1. `cd ~/apigee-workspace/apigee-opdk-playbook-workspace/apigee-opdk-playbook-installation-single-region`
1. `ansible-galaxy install -r install-edge-requirements.yml`
1. The first invocation should be: `ansible-playbook install-edge.yml --become --become-method=pbrun --tags=cache,ds,ms,rmp,qs,org`

# Installing Ansible on Windows
Please see the provided [Cygwin readme](README-CYGWIN.md) for instructions on installing Ansible on Windows.

# Installing Ansible
We use provide a [pip requirements file](ansible-pip-requirements.txt) for pip to install Ansible and its dependencies. This requires the
installation of pip on your system. Once pip is installed then the provided requirements file will
install Ansible and the necessary dependencies on your system.

# Pip Configuration
It may be necessary to configure pip to work with a repository that is internally managed. This can be accomplished by
updating the pip.conf file with the url of your managed repository. To accomplish this please follow these steps:

    mkdir ~/.pip
    touch ~/.pip/pip.conf
    vi ~/.pip/pip.conf

    [global]
    index-url = {{ pip_index_url }}

## Managed Pip Url
Please note that the url can be found in the your ~/apigee-workspace/apigee/custom-properties.yml. Please replace as follows:

    {{ pip_index_url }} >> "https://private.repository.com/api/pypi/Pypi-remote/simple"

# Pip Usage

* Upgrade your pip installation: `pip install -U pip`
* Install ansible and dependencies using the requirements file: `pip install -r ansible-pip-requirements.txt`

# Usage: Minimum Setup
Set up your a workspace at ~/apigee-workspace/ if you already have git and pip installed: 

    ansible-playbook setup-base.yml 
    
# Requirements
Requirements are declared in the [pip requirements file](ansible-pip-requirements.txt). The requirements
are listed as follows:
* Ansible 2.3.x
* Pip
* Boto, if using Ansible to manage EC2
* Markupsafe
* Paramiko

# Overview: Minimum Setup
This playbook will perform the following activities:

1. Scaffold required folders
1. Git clone of Ansible configuration file templates
1. Git clone of inventory file templates.
1. Create credentials.yml template file

# Default Workspace Folders
The default workspace folders can be modified by updating workspace-folders.yml.

# Default Github Repository List
The github repositories that you include can be modified by updating workspace-github-repos.yml

# Inventory File Semantics
A description of the how the [Inventory File](README-INVENTORY-FILE.md) should be setup.

# Scaffold required folders
This playbook will create the correct folders for you. Folder location can be modified by updating the [workspace-folders.yml](workspace-folders.yml).

## ~/.ansible/configurations
Sample configuration templates for single and multi-dc planets

## ~/.ansible/inventory
Sample configuration inventory template files that model the required
 group semantics used.

## ~/.apigee
Folder to contain credentials, downloaded logs, configs and maintain the edge license.txt file.
The [setup-apigee-license.yml](setup-apigee-license.yml) playbook will take a specified license file and
install it in the correct location for your this workspace.

## ~/apigee-workspace/apigee-opdk-playbook-workspace
Starter templates for playbooks. Use these playbooks as accelerators for your own.

## ~/apigee-workspace/apigee-odk-role-workspace
Workspace containing the roles used by the playbooks.

# Configure Credentials
Configuring of installation credentials is described in the [credentials readme](README-credentials.md).

# Configure Ansible
Ansible can be configured to make use of the above folders by providing an Ansible configuration file.
The Ansible configuration file can be generated in using the `setup-ansible-config.yml` playbook.

    ansible-playbook setup-ansible-config.yml
