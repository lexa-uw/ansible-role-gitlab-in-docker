GitLab in Docker
=========

Role for run GitLab in Docker container.

Requirements
------------

- ansible 2.5 or higher;
- docker is installed on host.

Role Variables
--------------

`gitlab_container_check_retries`
Count retries to check GitLab is started (_default: 60_)

`gitlab_container_check_delay`
Timeout for delay to check GitLab is started (_default: 5_)

`gitlab_container_options_*`
Options for run GitLab container. Default:
```yaml
gitlab_container_options_image: "gitlab/gitlab-ce:11.0.3-ce.0"
gitlab_container_options_links: []
gitlab_container_options_name: "gitlab"
gitlab_container_options_pull: "yes"
gitlab_container_options_ports: ["80:80"]
gitlab_container_options_recreate: false
gitlab_container_options_restart: false
gitlab_container_options_restart_policy: "on-failure"
gitlab_container_options_volumes: []
```

`gitlab_config`
GitLab configuration.

Add role to project:
----------------
Add role into your requirements(_requirements.yml_ for example):
```yaml
- src: lexa-uw.gitlab
  version: v2.0.0
  name: gitlab
```

Install role: `ansible-galaxy install -r ./requirements.yml --roles-path ./roles/`

Playbook example:
----------------

```yaml
- hosts: all
  vars_files:
    - vars/main.yml
  tasks:
    - name: Setup GitLab
      include_role:
        name: gitlab
```

Inside `vars/main.yml`
```yaml
gitlab_container_check_retries: 10
gitlab_container_check_delay: 15
gitlab_container_options_image: "gitlab/gitlab-ce:11.1.0-rc4.ce.0"
gitlab_container_options_name: "gitlab-my-company-name"
gitlab_container_options_ports: 
    - "{{ansible_default_ipv4.address}}:443:443"
    - "{{ansible_default_ipv4.address}}:80:80"
    - "{{ansible_default_ipv4.address}}:22:22"

gitlab_config: |
  external_url 'https://{{hostname}}'
  
  gitlab_rails['smtp_enable'] = true
  gitlab_rails['smtp_authentication'] = "login"
  gitlab_rails['smtp_address'] = "mailserver_host"
```

License
-------

MIT
