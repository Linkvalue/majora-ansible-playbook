# majora-ansible-playbook

Compilation of Ansible roles, optimized for Virtual Machines based on Ubuntu distributions for development environments.

These roles should not be used in production environments!



## Usage

Typical architecture to use these ansible playbook roles (with Vagrant for instance) would be (if you don't use [Ansible Galaxy](https://galaxy.ansible.com/intro#download-advanced)):

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
Vagrant.configure(2) do |config|

    config.vm.define :local, primary: true do |local|

        # Ubuntu version
        local.vm.box = "ubuntu/trusty64"

        # VirtualBox configuration
        local.vm.provider "virtualbox" do |vb|
            vb.customize ["modifyvm", :id, "--memory", 2048, "--cpus", "2"]
            vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
            vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
            vb.customize ["modifyvm", :id, "--usb", "off"]
            vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
        end

        # Network
        local.vm.hostname = "acme-project"
        local.vm.network "private_network", ip: "192.168.100.10"

        # SSH
        local.vm.network :forwarded_port, guest: 22, host: 3333, id: "ssh", disabled: true
        local.vm.network :forwarded_port, guest: 22, host: 3340, auto_correct: true

        # Utilities
        local.vm.network :forwarded_port, guest: 3306, host: 3306, id: "mysql"
        local.vm.network :forwarded_port, guest: 5432, host: 5432, id: "postgresql"
        local.vm.network :forwarded_port, guest: 27017, host: 27017, id: "mongodb"
        local.vm.network :forwarded_port, guest: 6379, host: 6379, id: "redis"
        local.vm.network :forwarded_port, guest: 9200, host: 9200, id: "elasticsearch"
        local.vm.network :forwarded_port, guest: 61680, host: 61680, id: "apollo"
        local.vm.network :forwarded_port, guest: 15672, host: 15672, id: "rabbitmq-management"

        # Share folders
        local.vm.synced_folder ".", "/var/www/AcmeProject/", id:"project-root", type: "nfs", mount_options: ["nolock,vers=3,udp,noatime,actimeo=1"]

        # Ansible provisionning
        local.vm.provision :ansible do |ansible|
            ansible.playbook = "ansible/app.yml"
            ansible.limit = "all"
        end

    end

end
```

`app.yml` as an entrypoint, should define which roles will be needed in the application, here is a typical example:

```yaml
# ansible/app.yml

- hosts: local
  sudo: yes
  roles:
    - etc-hosts
    - ssh-forward
    - common-packages
    - nginx
    - php
    - composer
    - postgresql
    - rabbitmq
    - rabbitmq-management
    - sass
    - nodejs
    - logstash
    - oh-my-zsh
  vars_files:
    - vars.yml
```

`vars.yml` as a "vars_file", should set (or override) useful variables for the application, here is a typical example:

```yaml
# ansible/vars.yml

ubuntu_version_name: 'trusty'

workspace: /var/www/AcmeProject

timezone: Europe/Paris

sites:
  - { name: 'acme-project', docroot: '/var/www/AcmeProject', server_name: 'acme-project.dev', template: 'vhost.symfony2.j2' }

postgresql:
  databases:
    - { name: 'acme-project' }
  users:
    - { name: 'acme-project' }

rabbitmq:
  users:
    - { name: 'admin' }

php_version: '5.6'
fpm: true                 # if php is run by php-fpm
# apache_mod_php: true    # if php is run by apache

etc_hosts_lines:
  - { ip: '192.168.100.12', hostname: 'my-other-vm-contains-redis-and-elasticsearch' }
  - { ip: '192.168.100.12', hostname: 'my-other-vm-project-used-in-acme-project.dev' }

logstash_config: >
  input {
  	redis {
  	  data_type => list
  	  host => my-other-vm-contains-redis-and-elasticsearch
  	  key => a_random_key
  	}
  }
  output {
    elasticsearch {
      index => my_index
      document_type => my_type
      hosts => ["my-other-vm-contains-redis-and-elasticsearch:9200"]
    }
  }
```



## Variables reference

All variables are facultative.

Take a look at each role's `defaults/main.yml` file to see all overridable variables. 

| Variable    | Description |
|:-----------:|-------------|
| php_version | Supported versions are `'5.4'` (with 'ubuntu/precise64' distrib), `'5.5'`, `'5.6'`, `'7.0'`. |




## Licenses

Roles listed below were developed by other happy folks and were just downloaded unchanged.

| Roles | License | Repository |
|:-----:|:-------:|------------|
| ruby | MIT | https://github.com/rvm/rvm1-ansible |
| redis | MIT | https://github.com/DavidWittman/ansible-redis |
