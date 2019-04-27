***Work in progress***

# [L'AirDuBois](https://www.lairdubois.fr) Ansible Playbook

Ansible playbook to deploy
[lairdubois.fr application](https://github.com/lairdubois/lairdubois). This
playbook can be read as an installation guide too.


## Setting up your Ansible environment

1. Create a python virtual environment: `python3 -m venv
   <name_of_your_virtualenv>`
2. Active your virtual environment: `source <path_to_your_virtualenv>/bin/activate`
3. Install Ansible through pip: `pip install ansible`
4. You should be able to run: `ansible-playbook --help`
5. Create the `hosts` file in the environment directory to configure the
   `lairdubois` group with something like:
```
[lairdubois]
lairdubois-<env name> ansible_host=<server_ip> ansible_user=<remote_user>
```

## Playbooks

The playbook is currently tested with a Debian 9.

### lairdubois.yml

This playbook sets up the complete infrastructure and applications required
for lairdubois application. The requirements are:
- the user used during the SSH login will own the application files
- the user used for login must be able to run sudo commands (add `-K` in the
  playbook arguments to provide the sudo password if needed)
- you must have access to the vault password to decrypt some variables.


```console
$ ansible-playbook -i environments/qa lairdubois.yml --ask-vault-pass
# OR
$ ansible-playbook -i environments/qa lairdubois.yml --vault-password-file=~/...
```

### lairdubois-create-user.yml

This playbook can be used to create a super user or normal user on the
platform. You will need at least to run it once to create a first super admin
user. It will prompt your for 4 informations:
- a username
- an email
- a password
- if the user should be a super admin

```console
$ ansible-playbook -i environments/qa lairdubois-create-user.yml --ask-vault-pass
# OR
$ ansible-playbook -i environments/qa lairdubois-create-user.yml --vault-password-file=~/...
```

## Environments

There are 3 defined environments: `dev`, `qa`, `prod`. Variables in the `all`
directory will be reused in all environments and can be seen as the default
values. You can customize by environment
any variable in their respective directory and files.

### DEV environment

The development environment is based on Vagrant. You can spin up a VM which
will be automatically provision with ansible with all the configuration
by running: `vagrant up`. You need to have Vagrant and VirtualBox installed
on your computer. You will be able to access the website under TLS with a
self-signed certificate at the address `https://localhost:3443`.

#### Helpful Vagrant commands

For all the commands you must have your shell in the directory containing
the `Vagrantfile`.

##### Start the VM

To start the VM:

```console
$ vagrant up
```

If the VM disk must be created, meaning you're running this command for the
first time or you destroyed the VM, the provisioning section of the `Vagrantfile`
will also be executed.

##### Provision the VM

If you need to provision again the VM manually, you should run:

```console
$ vagrant provision
```

##### Login

To login into the VM:

```console
$ vagrant ssh
```

##### Rebuild the image from scratch

To clean the current VM and rebuild the VM:

```console
$ vagrant destroy -f
$ vagrant up
```


### QA environment

This environment is a copy of the PROD environment to validate configuration
and settings with a production setup. This one should be used to provision
a VM from a cloud provider.

### PROD environment

*Not currently yet active.* This environment is used to deploy and manage
`lairdubois.fr` website.


## Ansible Vault

There is no hidden variables in the *vault* files. The variables in those
encrypted files are defined in the other variable files with the prefix
`vault_`. You can run a `git grep vault_` to find them all. This means the
following things:

- there isn't any variable defined in the playbooks which start with `vault_`
- variables that required to be kept secret are defined in the `vars.yml` files
  only and in the following format: `my_var_key: {{ vault_my_var_key }}`

### Vault password

The password is stored in the git repository using
[password store](http://www.passwordstore.org) application. You can retrieve it
with the command:

```console
$ pass ansible
```

### Edition

To edit a vault file, you have to go through the ansible vault command which
will display the file in a terminal editor like vi or nano depending on your
settings. Run the following command:

```console
$ ansible-vault edit environments/prod/group_vars/lairdubois/vault.yml --ask-vault-pass
# OR
$ ansible-vault edit environments/prod/group_vars/lairdubois/vault.yml --vault-password-file=~/...
```

You can look up other commands with: `ansible-vault --help`.


## Notes

### Debian testing
Shall we used that for the base system instead of the *stable* version to have
PHP 7.2 and more up-to-date packages (composer).


### DKIM keys
They are not yet generated/managed by the playbook.

## Links

- github application: https://github.com/lairdubois/lairdubois
- live application: https://www.lairdubois.fr
