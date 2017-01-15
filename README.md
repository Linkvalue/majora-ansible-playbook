# majora-ansible-playbook

Compilation of Ansible roles for Ubuntu 14.04 (for Ubuntu 12.04, use the old [v0.2.0 release](https://github.com/LinkValue/majora-ansible-playbook/tree/v0.2.0)).

**These roles should only be used to build development environments!**



## Usage

The easiest way to get started is to try our [Vagrant implementation](https://github.com/LinkValue/majora-ansible-vagrant).

*Pro tips:* If you don't want any troubles, you should always start by including the `common-packages` role in your machine.


## Variables reference (in alphabetical order)

All variables are optional.

Take a look at each role's `defaults/main.yml` file to see all overridable variables and their default values. 

| Variable    | Roles       | Description |
|:-----------:|:-----------:|-------------|
| apache_modules | apache | **ARRAY** <br>List of apache web server modules to install. <br>See default value [here](apache/defaults/main.yml). |
| apollo_version | apollo | Apollo version to install. <br>Default value is `'1.7.1'`. |
| capistrano_version | capistrano | Capistrano version to install using `gem`. <br>Default value is `'3.6.1'`. |
| common_packages | common-packages | **ARRAY** <br>List of packages to install using `apt-get install` command. Note that you'll probably prefer to use the `extra_packages` variable if you don't want to override the default `common_packages`. <br>See default value [here](common-packages/defaults/main.yml). |
| compass_version | compass | Compass version to install using `gem`. <br>Default value is `'1.0.3'`. |
| composer_github_token | composer | [GitHub personal access token](https://github.com/settings/tokens) to pass through the composer download limit. <br>Default value is `''`. |
| composer_packages | composer | **ARRAY** <br>List of composer packages to install globally using `composer global install` command. <br>Default value is `['symfony/var-dumper']` (which allows you to use `dump()` function in every PHP script). |
| elasticsearch_cerebro_version | elasticsearch-cerebro | Cerebro (elasticsearch management plugin) version to install. <br>Default value is `'0.4.2'`. |
| elasticsearch_cerebro_port | elasticsearch-cerebro | Port on which Cerebro web interface will be accessible. <br>Default value is `'9000'`. |
| elasticsearch_heap_size | elasticsearch | Java Virtual Machine heap size usage for elasticsearch ("-Xms" and "-Xmx" values). <br>Default value is `'512m'`. |
| elasticsearch_network_host | elasticsearch | Elasticsearch network host ("network.host" configuration value). <br>Default value is `'0.0.0.0'`. |
| elasticsearch_node_name | elasticsearch | Elasticsearch node name ("node.name" configuration value). <br>Default value is `'majora-ansible'`. |
| elasticsearch_version | elasticsearch | Elasticsearch version to install. See [this](https://www.elastic.co/downloads/past-releases) for supported versions. <br>Default value is `'5.1.2'`. |
| etc_hosts_lines | etc-hosts | **ARRAY** <br>[See "Complex variables section"](#complex-variables). <br>See default value [here](etc-hosts/defaults/main.yml). |
| extra_packages | common-packages | **ARRAY** <br>List of packages to install (after `common_packages`) using `apt-get install` command. <br>Default value is `[]`. |
| kibana_elasticsearch_url | kibana | Elasticsearch URL on which Kibana should connect. <br>Default value is `'http://localhost:9200'`. |
| kibana_server_host | kibana | Kibana server host. <br>Default value is `'0.0.0.0'`. |
| kibana_server_name | kibana | Kibana server name. <br>Default value is `'majora-ansible'`. |
| kibana_server_port | kibana | Port on which Kibana web interface will be accessible. <br>Default value is `'5601'`. |
| kibana_version | kibana | Kibana version to install. <br>Default value is `'5.1.2'`. |
| language | common-packages | Value for "locales" related environment variables (i.e. `LANG`, `LANGUAGE`, `LC_ALL`). <br>Default value is `en_US.UTF-8`. |
| logstash_config | logstash | **MULTI-LINES** <br>Allow to set up the logstash configuration file content. <br>See default value [here](logstash/defaults/main.yml). |
| logstash_version | logstash | Logstash version to install. <br>Default value is `'5.1.2'`. |
| lua_version | lua | Lua version to install. <br>Default value is `'5.1'`. |
| lua_packages | lua | **ARRAY** <br>List of packages to install using `apt-get install` command. <br>See default value [here](lua/defaults/main.yml). |
| luarocks_packages | lua | **ARRAY** <br>List of packages to install using `luarocks install` command. <br>See default value [here](lua/defaults/main.yml). |
| main_group | composer/oh-my-zsh | Main group (main user's group). <br>Default value is `vagrant`. |
| main_user | composer/oh-my-zsh | Main user (the one you'll use in the provisioned machine). <br>Default value is `vagrant`. |
| main_user_home_dir | composer/oh-my-zsh | Main user's home directory. <br>Default value is `/home/vagrant`. |
| mongodb | mongodb | **OBJECT** <br>[See "Complex variables section"](#complex-variables). <br>Default value is `{}`. |
| mysql | mysql | **OBJECT** <br>[See "Complex variables section"](#complex-variables). <br>Default value is `{}`. |
| nginx_package | nginx | NGINX package to install using `apt-get install` command. <br>Default value is `'nginx'`. |
| node_version | nodejs | NodeJS version to install. This value will be append to `https://deb.nodesource.com/setup_`. <br>Default value is `6.x`. |
| npm_packages | nodejs | **ARRAY** <br>List of npm packages to install globally using `npm install -g` command. <br>See default value [here](nodejs/defaults/main.yml). |
| pip_packages | python | **ARRAY** <br>List of python packages to install using `pip install` command (for each `python_versions`). <br>Default value is `[]`. |
| phantomjs_version | phantomjs | PhantomJS version to install. <br>Default value is `1.9.8`. |
| php55_extensions | php | **ARRAY** <br>List of apt-get packages which will be installed if `php_versions` contains `5.5`. <br>See default value [here](php/defaults/main.yml). |
| php56_extensions | php | **ARRAY** <br>List of apt-get packages which will be installed if `php_versions` contains `5.6`. <br>See default value [here](php/defaults/main.yml). |
| php70_extensions | php | **ARRAY** <br>List of apt-get packages which will be installed if `php_versions` contains `7.0`. <br>See default value [here](php/defaults/main.yml). |
| php71_extensions | php | **ARRAY** <br>List of apt-get packages which will be installed if `php_versions` contains `7.1`. <br>See default value [here](php/defaults/main.yml). |
| php_disable_xdebug_cli | php | If set to `false`, xdebug php extension will be enabled in PHP CLI (which considerably reduces "composer" speed). <br>Default value is `true`. |
| php_memory_limit | php | Defines [`memory_limit` setting in `php.ini`](http://php.net/manual/en/ini.core.php#ini.memory-limit). <br>Default value is `'2G'`. |
| php_versions | php/nginx | **ARRAY** <br>Supported versions are `'5.5'`, `'5.6'`, `'7.0'`, `'7.1'`. The `php` shell command, the `sites[x].fastcgi_pass` default value for nginx templates and the `php apache module` will all use the first php version found in this array. To switch apache's php versions, you'll have to dismiss current php module, and enable the wanted one (e.g. to stop using php5.6 and start using php7.0: `sudo a2dismod php5.6 && sudo a2enmod php7.0 && sudo service apache2 restart`). <br>Default value is `['7.0']`. |
| php_xdebug_remote_port | php | Port on which you can listen to start a remote debugging connection. <br>Default value is `'9000'`. |
| postgresql | postgresql | **OBJECT** <br>[See "Complex variables section"](#complex-variables). <br>Default value is `{}`. |
| postgresql_version | postgresql | PostgreSQL version to install. <br>Default value is `'9.4'`. |
| python_versions | python | **ARRAY** <br>Supported versions are `'2.7'`, `'3.5'`. <br>Default value is `['3.5']`. |
| python_is_first_python_versions | python | If set to `true`, the `python` and `pip` shell commands will all use the first python version found in `python_versions` array (it may lead to unexpected issues because Ansible itself use `python`). <br>Default value is `false`. |
| rabbitmq | rabbitmq | **OBJECT** <br>[See "Complex variables section"](#complex-variables). <br>Default value is `{}`. |
| sass_version | sass | Sass version to install using `gem`. <br>Default value is `'3.4.22'`. |
| sites | apache/nginx | **ARRAY** <br>[See "Complex variables section"](#complex-variables). <br>Default value is `[]`. |
| timezone | common-packages | Value for timezone system configuration. <br>Default value is `'UTC'`. |
| wkhtmltopdf_version | wkhtmltopdf | WkhtmlToPDF version (short) to install (`wkhtmltopdf_version_full` must be set  accordingly). <br>Default value is `'0.12'`. |
| wkhtmltopdf_version_full | wkhtmltopdf | WkhtmlToPDF version (full) to install (`wkhtmltopdf_version` must be set accordingly). <br>Default value is `'0.12.2.1'`. |
| workspace | oh-my-zsh | Full path directory where you'll land on session start. It will also create the `www` alias which will `cd` to this path. <br>Default value is `'/var/www'`. |
| zsh_additional_commands | oh-my-zsh | **MULTI-LINES** <br>Allow to set additional commands which will be executed each time you open a ZSH terminal. <br>Default value is `export IS_VM=true`. |
| zsh_disable_auto_update | oh-my-zsh | If set to `false`, `oh-my-zsh` will suggest you to update it when new releases are available. <br>Default value is `true`. |
| zsh_editor | oh-my-zsh | Default terminal editor. <br>Default value is `'vim'`. |
| zsh_plugins | oh-my-zsh | **ARRAY** <br>List of `oh-my-zsh` plugins to install. Supported plugins are located in `~/.oh-my-zsh/plugins/` inside the VM. <br>See default value [here](oh-my-zsh/defaults/main.yml). |
| zsh_theme | oh-my-zsh | Supported themes for `oh-my-zsh` are located in `~/.oh-my-zsh/themes/` inside the VM. <br>Default value is `'robbyrussell'`. |



### Complex variables

Here is a list of the "object variables" which require a deeper look in their own role to fully understand what they do:

| Variable | Role | Link | Description |
|:---------:|:-------:|:-------:|:-------:|
| etc_hosts_lines | etc-hosts | [Examples](etc-hosts/defaults/main.yml) / [How it is used](etc-hosts/tasks/main.yml) | Add lines to the VM `/etc/hosts` file. |
| mongodb | mongodb | [Examples](mongodb/defaults/main.yml) / [How it is used](mongodb/tasks/main.yml) | Create `users`. |
| mysql | mysql | [Examples](mysql/defaults/main.yml) / [How it is used](mysql/tasks/main.yml) | Create `databases` and `users`. |
| postgresql | postgresql | [Examples](postgresql/defaults/main.yml) / [How it is used](postgresql/tasks/main.yml) | Create `databases` and `users`. |
| rabbitmq | rabbitmq | [Examples](rabbitmq/defaults/main.yml) / [How it is used](rabbitmq/tasks/main.yml) | Create `users`. |
| sites | apache/nginx | [See "Nginx/Apache configuration" section below](#nginxapache-server-vhosts-configuration) | Create "virtual hosts" for your web applications. |



## Nginx/Apache server vhosts configuration

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

| Templates | Roles |
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
