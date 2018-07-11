Run tests
=========

## Install dependencies
```bash
ansible-galaxy install -r ./tests/requirements.yml --roles-path ./tests/roles/
```

## Run tests
```bash
ANSIBLE_ROLES_PATH=./../ ansible-playbook ./tests/test.yml -i ./tests/inventory
```
