***Work in progress***

# lairdubois-ansible
Ansible playbook to deploy lairdubois.fr infrastructure

## Setting up your Ansible environment

1. Create a python virtual environment: `python3 -m venv <name_of_your_virtualenv>`
2. Active your virtual environment: `. <path_to_your_virtualenv>/bin/activate`
3. Install Ansible through pip: `pip install ansible`
4. You should be able to run: `ansible-playbook --help`


## Run the playbook

```
ansible-playbook -i environment/qa lairdubois.yml
```


## Notes

### Debian testing
Shall we used that for the base system instead of the *stable* version to have PHP 7.2 and more up-to-date packages (composer).
