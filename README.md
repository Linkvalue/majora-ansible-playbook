# majora-ansible-playbook

Compilation of Ansible roles, optimized for Virtual Machines based on Ubuntu distributions for development environments.

These roles should not be used in production environments!



## Usage

The easiest way to get started is to try our [Vagrant implementation](https://github.com/LinkValue/majora-ansible-vagrant).



## Variables reference

All variables are facultative.

Take a look at each role's `defaults/main.yml` file to see all overridable variables. 

| Variable    | Description |
|:-----------:|-------------|
| php_disable_xdebug_cli | If set to `true`, xdebug php extension won't be enabled in PHP CLI (which is a great improvement while using "composer" but still has drawbacks in other fields). <br>Default value is `false`. |
| php_version | Supported versions are `'5.4'` (with 'ubuntu/precise64' distrib), `'5.5'`, `'5.6'`, `'7.0'` (currently only supported with `nginx/fpm`). <br>Default value is `'5.6'`. |



## Dependencies

Here is a list of roles which depend on external roles.

See [LinkValue/majora-ansible-vagrant](https://github.com/LinkValue/majora-ansible-vagrant) to know how to easily fullfil these dependencies.

| Role | License | Dependencies |
|:----:|:-------:|--------------|
| sass       | MIT | [rvm_io.rvm1-ruby](https://github.com/rvm/rvm1-ansible) as `ruby` |
| capistrano | MIT | [rvm_io.rvm1-ruby](https://github.com/rvm/rvm1-ansible) as `ruby` |
