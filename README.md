#Puppet Orchestration Using Ansible

Right now, the only way to run this repo is to spin up vagrant instances to test ansible against. The playbooks setup a Puppet Enterprise master and two agents.

**Requirements:** You will need to [install ansible](http://docs.ansible.com/ansible/intro_installation.html) before using this repo. Virtualbox and Vagrant are also required.

##Vagrant Testing

###1. Clone this repo

###2. ```cd puppet-ansible && vagrant up```

###3. Setup ssh keys using ansible to make things easier:

```ansible-playbook -c paramiko -i setup/hosts setup/setup.yml --ask-pass --sudo```

**NOTE:** If you destroy the vagrant instances, re-create them, and re-run this playbook, you will get errors unless you remove the vagrant IP addresses from your laptop's known hosts file.

You'll be prompted for a password. It is 'vagrant.' You will then be prompted to type yes three times, much like you would when initializing a new ssh connection to a node.

###4. Setup the master:

```ansible-playbook -i hosts master.yml```

###5. Setup the agents:

```ansible-playbook -i hosts agent.yml --extra-vars "puppetserver=192.168.33.10"```

##Directories

###Vagrantfile
A simple one nabbed from the [ansible tutorial](https://github.com/leucos/ansible-tuto), changed slightly to include more memory due to PE needing more resources.

###agent.yml
Playbook to install PE agents. Calls common and puppetagent role.

###group_vars
Variables that are set per group. In the world of ansible, "group" means groups that are set in the host inventory file.

###host_vars
Variables set per host.

###hosts
Ansible's host inventory file.

###master.yml
Playbook to install PE master. Calls common and puppetmaster role.

###roles
Contains playbooks and files specific to roles. Right now we have common, puppetagent and puppetmaster.

###setup
Playbooks to do some basic setup on the vagrant instances.

####To do:

1. Make POSS/puppet apply an option
2. AWS setup

####More resources:
[ansible tutorial](https://github.com/leucos/ansible-tuto): I highly recommend going through this before using this repo, although it's not necessary.
[ansible documentation](http://docs.ansible.com/ansible/index.html)