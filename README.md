# majora-ansible-playbook

Compilation of ansible roles to use in majora projects

## Usage

Typical architecture to use these ansible playbook roles (with Vagrant for instance) would be:

```bash
    AcmeProject
    |--- ansible
    |    |--- roles
    |    |    |--- [THIS REPOSITORY CONTENT]
    |    |--- app.yml
    |    +--- vars.yml
    +--- Vagrantfile
```

Where `app.yml` is the playbook entrypoint define in the `Vagrantfile` such as:

```ruby
# Ansible provisionning from Vagrantfile
config.vm.provision :ansible do |ansible|
    ansible.playbook = "ansible/app.yml"
    ansible.limit = "all"
end
```

`app.yml` as an entrypoint, should define which roles will be needed in the application, here is a typical example:

```yaml
# ansible/app.yml

- hosts: local
  sudo: yes
  roles:
    - ssh_forward
    - common
    - ruby
    - sass
    - capistrano
    - nginx
    - php-fpm
    - nodejs
    - java
    - elasticsearch
    - mysql
    - redis
  vars_files:
    - vars.yml
```

`vars.yml` as a "var_file", should set (or override) useful variables for the application, here is a typical example:

```yaml
# ansible/vars.yml

workspace: /var/www/AcmeProject

sites:
  - { name: 'acme-project', docroot: '/var/www/AcmeProject', server_name: 'acme-project.dev' }

mysql:
  databases:
    - { name: 'acme-project' }
  users:
    - { name: 'acme-project', host: '%' }
  bind_address: 0.0.0.0

php_version: "7.0"
```

## Variables reference

| Variable    | Description                                       |
|:-----------:|---------------------------------------------------|
| php_version | Supported versions are `"5.5"`, `"5.6"`, `"7.0"`. |
