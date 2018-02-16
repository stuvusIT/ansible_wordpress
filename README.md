# Wordpress ansible

This role deploys a wordpress instance 


## Requirements

This role requires the [php-fpm role](https://github.com/stuvusIT/php-fpm), the [nginx role](https://github.com/stuvusIT/nginx) and the [maria db role](https://github.com/stuvusIT/mariadb)


## Role Variables

### General

| Name                         | Required/Default   | Description                                                                   |
|------------------------------|:------------------:|-------------------------------------------------------------------------------|
| `wordpress_mysql_password`   | :heavy_check_mark: | The password for the mysql database                                           |
| `wordpress_default_user`     | `admin`            | This is the user that is created on setup                                     |
| `wordpress_default_password` | :heavy_check_mark: | The password that is used for the default user                                |
| `wordpress_default_email`    | :heavy_check_mark: | The email address for the default user                                        |
| `wordpress_plugin_merge`     | `true`             | Default behaviour when using plugin options                                   |
| `wordpress_plugins`          | `[]`               | List of plugins to install and options to set. For more information see below |

### Plugins

| Name      | Required/Default               | Description                                                             |
|-----------|--------------------------------|-------------------------------------------------------------------------|
| `name`    | :heavy_check_mark:             | The name(slug) of the plugin to be installed.                           |
| `merge`   | `{{ wordpress_plugin_merge }}` | Setting if the options should be merged or overwritten                  |
| `options` | :heavy_check_mark:             | Dict with options. The key is the same key as in the wordpress database |

### Design

| Name                        | Required/Default  | Description                                                                                                                                                        |
|-----------------------------|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `wordpress_template`        | `twentyseventeen` | The name of the wordpress template being in use                                                                                                                    |
| `wordpress_import_template` | `false`           | If this is true ansible will try to extract a zip file from   `files/{{inventory_hostname}}/{{ wordpress_template }}.zip ` and install it in the wordpress instace |
| `wordpress_stylesheet`      | `twentyseventeen` | What stylesheet should be used for the wordpress instance                                                                                                          |

### Import wordpress instance

| Name                          | Required/Default | Description                                                                                                                                 |
|-------------------------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| `wordpress_import_wp_content` | `false`          | If this is true ansible will try to copy the wp-content directory over to the remote host from   `files/{{inventory_hostname}}/wp-content/` |
| `wordpress_import_db_file`    | `false`          | If this is true ansible will try to import an sql file from `files/{{inventory_hostname}}/wordpress.sql`                                    |

### Mail

| Name                         | Required/Default    | Description                  |
|------------------------------|:--------------------|------------------------------|
| `wordpress_mailserver_url`   | `mail.example.com`  | url of the mailserver        |
| `wordpress_mailserver_login` | `login@example.com` | Login name for the mailserer |
| `wordpress_mailserver_pass`  | `password`          | Password for the mailserver  |
| `wordpress_mailserver_port`  | `110`               | Mailserver port              |

## Dependencies

The php-fpm role needs to be up and running and a nginx server should be running and serving files from  `/var/www/wordpress` also it needs a maria db instance with the socket running under  `/var/run/mysqld/mysqld.sock`


## Example Playbook

```yml
- hosts: wordpress
  roles:
    - role: wordpress
      wordpress_mysql_password: password
      wordpress_default_password: password
      wordpress_default_email: admin@example.com
```

### Result

Running wordpress instance.

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).


## Author Information

 * [Fritz Otlinghaus(Scriptkiddi)](https://github.com/Scriptkiddi) _fritz.otlinghaus@stuvus.uni-stuttgart.de_
