***Work in progress***

# lairdubois-ansible
Ansible playbook to deploy lairdubois.fr infrastructure

## Setting up your Ansible environment

1. Create a python virtual environment: `python3 -m venv <name_of_your_virtualenv>`
2. Active your virtual environment: `. <path_to_your_virtualenv>/bin/activate`
3. Install Ansible through pip: `pip install ansible`
4. You should be able to run: `ansible-playbook --help`
5. Create a file `lairdubois.hosts` with the host definition based on this template:

```
[lairdubois]
lairdubois ansible_host=<IP_ADDRESS_OF_THE_SERVER>
```

## Run the playbook

```
ansible-playbook -i lairdubois.hosts lairdubois.yml
```
