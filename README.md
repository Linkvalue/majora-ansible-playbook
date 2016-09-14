# majora-ansible-playbook

Compilation of Ansible roles for Ubuntu 14.04 (for Ubuntu 12.04, use the old [v0.2.0 release](https://github.com/LinkValue/majora-ansible-playbook/tree/v0.2.0)).

**These roles should only be used to build development environments!**



## Usage

The easiest way to get started is to try our [Vagrant implementation](https://github.com/LinkValue/majora-ansible-vagrant).



## Variables reference (in alphabetical order)

All variables are optional.

Take a look at each role's `defaults/main.yml` file to see all overridable variables and their default values. 

| Variable    | Roles       | Description |
|:-----------:|:-----------:|-------------|
| common_packages | common-packages | **ARRAY** <br>List of common packages to install using `apt-get install` command. <br>See default value [here](common-packages/defaults/main.yml). |
| extra_packages | common-packages | **ARRAY** <br>List of extra packages to install using `apt-get install` command. <br>Default value is `[]`. |
| language | common-packages | Value for "locales" related environment variables (i.e. `LANG`, `LANGUAGE`, `LC_ALL`). <br>Default value is `en_US.UTF-8`. |
| pip_packages | python | **ARRAY** <br>List of python packages to install using `pip install` command (for each python_versions). <br>Default value is `[]`. |
| php55_extensions | php | **ARRAY** <br>List of apt-get packages which will be installed if `php_versions` contains `5.5`. <br>See default value [here](php/defaults/main.yml). |
| php56_extensions | php | **ARRAY** <br>List of apt-get packages which will be installed if `php_versions` contains `5.6`. <br>See default value [here](php/defaults/main.yml). |
| php70_extensions | php | **ARRAY** <br>List of apt-get packages which will be installed if `php_versions` contains `7.0`. <br>See default value [here](php/defaults/main.yml). |
| php_disable_xdebug_cli | php | If set to `false`, xdebug php extension will be enabled in PHP CLI (which considerably reduces "composer" speed). <br>Default value is `true`. |
| php_memory_limit | php | Defines [`memory_limit` setting in `php.ini`](http://php.net/manual/en/ini.core.php#ini.memory-limit). <br>Default value is `'2G'`. |
| php_versions | php | **ARRAY** <br>Supported versions are `'5.5'`, `'5.6'`, `'7.0'`. The `php` shell command, the `sites[x].fastcgi_pass` default value for nginx templates and the `php apache module` will all use the first php version found in this array. To switch apache's php versions, you'll have to dismiss current php module, and enable the wanted one (e.g. to stop using php5.6 and start using php7.0: `sudo a2dismod php5.6 && sudo a2enmod php7.0 && sudo service apache2 restart`). <br>Default value is `['5.6']`. |
| php_xdebug_remote_port | php | Port on which you can listen to start a remote debugging connection. <br>Default value is `'9000'`. |
| python_versions | python | **ARRAY** <br>Supported versions are `'2.7'`, `'3.5'`. <br>Default value is `['3.5']`. |
| python_is_first_python_versions | python | If set to `true`, the `python` and `pip` shell commands will all use the first python version found in `python_versions` array (it may lead to unexpected issues because Ansible itself use `python`). <br>Default value is `false`. |
| sites | apache/nginx | **ARRAY** <br>[See "Nginx/Apache section"](#nginxapache-server-configuration). <br>Default value is `[]`. |
| timezone | common-packages | Value for timezone system configuration. <br>Default value is `UTC`. |
| zsh_additional_commands | oh-my-zsh | **MULTI-LINES** <br>Allow to set additional commands which will be executed each time you open a ZSH terminal. <br>Default value is `export IS_VM=true`. |
| zsh_editor | oh-my-zsh | Default terminal editor. <br>Default value is `'vim'`. |
| zsh_theme | oh-my-zsh | Supported themes for `oh-my-zsh` are located in `~/.oh-my-zsh/themes/` inside the VM. <br>Default value is `'robbyrussell'`. |



## Nginx/Apache server configuration

You can provision `nginx` or `apache` server configuration using the `sites` variable like the following examples:

```yml
# nginx/apache configuration for 1 simple PHP project and 2 Symfony projects
sites:
  - { name: 'php-project', server_name: 'php.dev', template: 'vhost.php.j2' } # These are the 3 required variables for a site definition
  - { name: 'acme-sf-project', server_name: 'acme.dev', template: 'vhost.symfony.j2', docroot: '/var/www/acme-sf-project/web' } # server_alias is "*.{{server_name}}" by default
  - { name: 'emca-sf-project', server_name: 'emca.dev', template: 'vhost.symfony.j2', server_alias: '*.local.custom.emca.dev' } # docroot will point to "/var/www/{{name}}/web" by default for symfony template
```

```yml
# nginx configuration for a NodeJS project and a Wordpress project
sites:
  - { name: 'NodeJS', server_name: 'nodejs.dev', template: 'vhost.nodejs.j2' }
  - { name: 'Wordpress', server_name: 'wordpress.dev', template: 'vhost.wordpress.j2', fastcgi_pass: 'unix:/run/php/php7.0-fpm.sock' } # docroot will point to "/var/www/{{name}}" by default for wordpress template
```

Take a look into `nginx` or `apache` directories to see exactly how you can use our different configuration templates.

| Templates | Servers |
|:---------:|:-------:|
| vhost.php.j2 | apache/nginx |
| vhost.magento.j2 | nginx |
| vhost.nodejs.j2 | nginx |
| vhost.silex.j2 | nginx |
| vhost.symfony.j2 | apache/nginx |
| vhost.wordpress.j2 | nginx |
| vhost.zend.j2 | nginx |



## Dependencies

Here is a list of roles which depend on external roles.

See [LinkValue/majora-ansible-vagrant](https://github.com/LinkValue/majora-ansible-vagrant) to know how to easily fullfil these external dependencies.

| Role | License | Dependencies |
|:----:|:-------:|--------------|
| sass | MIT | [rvm_io.rvm1-ruby](https://github.com/rvm/rvm1-ansible) as `ruby` |
| capistrano | MIT | [rvm_io.rvm1-ruby](https://github.com/rvm/rvm1-ansible) as `ruby` |
