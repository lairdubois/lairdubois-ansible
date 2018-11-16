***Work in progress***

# [L'AirDuBois](https://www.lairdubois.fr) Ansible Playbook

Ansible playbook to deploy
[lairdubois.fr application](https://github.com/lairdubois/lairdubois). This
playbook can be read as an installation guide too.

Feel free to try it and improve it.


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

The base template for the template should be a Debian 9.

### lairdubois.yml

This playbook sets up the complete infrastructure and applications required
for lairdubois application. The requirements are:
- the user used during the SSH login will own the application files
- the user used for login must be able to run sudo commands (add `-K` in the
  playbook arguments to provide the sudo password if needed)

```bash
ansible-playbook -i environments/qa lairdubois.yml --vault-password-file=~/...
```

### lairdubois-create-user.yml

This playbook can be used to create a super user or normal user on the
platform. You will need at least to run it once to create a first super admin
user. It will prompt your for 4 informations:
- a username
- an email
- a password
- if the user should be a super admin

```bash
ansible-playbook -i environments/qa lairdubois-create-user.yml --vault-password-file=~/...
```

## Environments

There are 3 defined environments: dev, qa, prod. Variables in the `all`
directory will be reused in all environments. You can customize by environment
any variable in their respective directory and files.

There is no hidden variables in the *vault* files. The variables in those
encrypted files are defined in the other variable files with the prefix
`vault_`. You can run a `git grep vault_` to find them all.


## Notes

### Debian testing
Shall we used that for the base system instead of the *stable* version to have
PHP 7.2 and more up-to-date packages (composer).


### DKIM keys
They are not yet generated/managed by the playbook.

## Links

- github application: https://github.com/lairdubois/lairdubois
- live application: https://www.lairdubois.fr
