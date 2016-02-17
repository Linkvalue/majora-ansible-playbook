# majora-ansible-playbook

Compilation of Ansible roles, optimized for Virtual Machines based on Ubuntu distributions for development environments.

These roles should not be used in production environments!



## Usage

The easiest way to get started is to try our [Vagrant implementation](https://github.com/LinkValue/majora-ansible-vagrant).



## Variables reference (sorted in alphabetical order)

All variables are facultative.

Take a look at each role's `defaults/main.yml` file to see all overridable variables and their default values. 

| Variable    | Roles       | Description |
|:-----------:|:-----------:|-------------|
| php_disable_xdebug_cli | php | If set to `false`, xdebug php extension will be enabled in PHP CLI (which reduces "composer" speed considerably). <br>Default value is `true`. |
| php_version | php | Supported versions are `'5.4'` (with 'ubuntu/precise64' distrib), `'5.5'`, `'5.6'`, `'7.0'` (currently only supported with `nginx/fpm`). <br>Default value is `'5.6'`. |
| sites | apache/nginx | **ARRAY** <br>[See "Nginx/Apache section"](#nginxapache-server-configuration). <br>Default value is `[]`. |
| zsh_additional_commands | oh-my-zsh | **MULTI-LINES** <br>Allow to set additional commands which will be executed each time you open a ZSH terminal. <br>Default value is `export IS_VM=true`. |
| zsh_editor | oh-my-zsh | Default terminal editor. <br>Default value is `'vim'`. |
| zsh_theme | oh-my-zsh | Supported themes for `oh-my-zsh` are located in `~/.oh-my-zsh/themes/` inside the VM. <br>Default value is `'robbyrussell'`. |



## Nginx/Apache server configuration

You are able to provision `nginx` or `apache` server configuration using the `sites` Ansible variable like the following examples:

```yml
# nginx/apache configuration for 2 symfony projects
sites:
  - { name: 'acme-sf-project', server_name: 'acme.dev', template: 'vhost.symfony.j2', docroot: '/var/www/acme-sf-project/web' } # server_alias is "*.{{server_name}}" by default
  - { name: 'emca-sf-project', server_name: 'emca.dev', template: 'vhost.symfony.j2', server_alias: '*.local.custom.emca.dev' } # docroot will point to "/var/www/{{name}}/web" by default for symfony template
```

```yml
# nginx configuration for a NodeJS project and a Wordpress project
sites:
  - { name: 'NodeJS', server_name: 'nodejs.dev', template: 'vhost.proxy-node.j2' }
  - { name: 'Wordpress', server_name: 'wordpress.dev', template: 'vhost.wordpress.j2' } # docroot will point to "/var/www/{{name}}" by default for wordpress template
```

Take a look into `nginx` or `apache` directories to see exactly how you can use our different configuration templates.

| Templates | Servers | Description |
|:---------:|:-------:|-------------|
| vhost.default.j2 | apache/nginx |  |
| vhost.magento.j2 | nginx |  |
| vhost.proxy-node.j2 | nginx |  |
| vhost.silex.j2 | nginx |  |
| vhost.symfony.j2 | apache/nginx |  |
| vhost.wordpress.j2 | nginx |  |
| vhost.zend.j2 | nginx |  |



## Dependencies

Here is a list of roles which depend on external roles.

See [LinkValue/majora-ansible-vagrant](https://github.com/LinkValue/majora-ansible-vagrant) to know how to easily fullfil these dependencies.

| Role | License | Dependencies |
|:----:|:-------:|--------------|
| sass       | MIT | [rvm_io.rvm1-ruby](https://github.com/rvm/rvm1-ansible) as `ruby` |
| capistrano | MIT | [rvm_io.rvm1-ruby](https://github.com/rvm/rvm1-ansible) as `ruby` |
