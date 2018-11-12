***Work in progress***

# lairdubois-ansible
Ansible playbook to deploy lairdubois.fr infrastructure

## Setting up your Ansible environment

1. Create a python virtual environment: `python3 -m venv <name_of_your_virtualenv>`
2. Active your virtual environment: `. <path_to_your_virtualenv>/bin/activate`
3. Install Ansible through pip: `pip install ansible`
4. You should be able to run: `ansible-playbook --help`
5. Create the `hosts` file in the environment directory to configure the `lairdubois` group with something like:
```
[lairdubois]
lairdubois-qa ansible_host=<server_ip> ansible_user=<remote_user>
```

## Run the playbook

```
ansible-playbook -K -c local -i host.local playbook.yml --vault-password-file=~/...
```


## Notes

### Debian testing
Shall we used that for the base system instead of the *stable* version to have PHP 7.2 and more up-to-date packages (composer).

### ACME / DNS
For now, the file for the API key used by gandi are not included in the repository and must be added to the file `~/.acme.sh/account.conf` under the key `GANDI_LIVEDNS_KEY=...`
