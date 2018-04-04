GitLab in Docker
=========

Role for run GitLab in Docker container.

Requirements
------------

- ansible 2.5 or higher;
- docker is installed on host.

Role Variables
--------------
`gitlab_dir`
Directory where data will be stored (_default: "/tmp/gitlab"_)

`gitlab_container_check_url`
Url where GitLab will be checked to run status. (_optional_)

`gitlab_container_check_retries`
Count retries to check GitLab is started (_default: 60_)

`gitlab_container_check_delay`
Timeout for delay to check GitLab is started (_default: 5_)

`gitlab_container`
Options for run GitLab container. Default:
```yaml
gitlab_container:
  image: "gitlab/gitlab-ce:10.6.2-ce.0"
  name: "gitlab"
  restart: false
  links: []
  ports:
    - "80:80"
  volumes:
    - "{{gitlab_dir}}/etc/gitlab:/etc/gitlab"              # configs
    - "{{gitlab_dir}}/var/opt/gitlab:/var/opt/gitlab"      # data
    - "{{gitlab_dir}}/var/log/gitlab:/var/log/gitlab"      # logs
```

`gitlab_config`
GitLab configuration.

Add role to project:
----------------
Add role into your requirements(_requirements.yml_ for example):
```yaml
- src: lexa-uw.gitlab
  version: v1.0.0
  name: gitlab
```

Install role: `ansible-galaxy install -r ./requirements.yml --roles-path ./roles/`

Playbook example:
----------------

```yaml
- hosts: all
  vars_files:
    - vars/main.yml
  roles:
    - { role: gitlab }
```

Inside `vars/main.yml`
```yaml
hostname: gitlab.example.com

gitlab_dir: "/tmp/gitlab"
gitlab_container_check_url: "http://{{ hostname }}:80"
gitlab_container_check_retries: 10
gitlab_container_check_delay: 15
gitlab_container:
  image: "gitlab/gitlab-ce:10.6.2-ce.0"
  name: "gitlab_{{hostname}}"
  links:
    - "mailserver:mail"
  ports:
    - "{{ansible_default_ipv4.address}}:443:443"
    - "{{ansible_default_ipv4.address}}:80:80"
    - "{{ansible_default_ipv4.address}}:22:22"
  volumes:
    - "{{gitlab_dir}}/mnt/etc/gitlab:/etc/gitlab"              # configs
    - "{{gitlab_dir}}/mnt/var/opt/gitlab:/var/opt/gitlab"      # data
    - "{{gitlab_dir}}/mnt/var/log/gitlab:/var/log/gitlab"      # logs

gitlab_config: |
  external_url 'https://{{hostname}}'
  
  gitlab_rails['smtp_enable'] = true
  gitlab_rails['smtp_authentication'] = "login"
  gitlab_rails['smtp_address'] = "mailserver_host"
```

License
-------

MIT
