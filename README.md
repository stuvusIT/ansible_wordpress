# Wordpress ansible

This role deploys a wordpress instance 


## Requirements

This role requires the [php-fpm role](https://github.com/stuvusIT/php-fpm), the [nginx role](https://github.com/stuvusIT/nginx) and the [maria db role](https://github.com/stuvusIT/mariadb)


## Role Variables

### General

| Name                 | Required                 | Default        | Description                                            |
|----------------------|:------------------------:|----------------|----------------------------------------------------------------|
| `wordpress_mysql_password`      | :heavy_check_mark:       |         | The password for the mysql database                             |
| `wordpress_default_user` | :heavy_multiplication_x:       | `admin`        | This is the user that is created on setup |
| `wordpress_default_password`      | :heavy_check_mark:|                | The password that is used for the default user |
| `wordpress_default_email`      | :heavy_check_mark:|                | The email address for the default user |


### Design

| Name                 | Required                 | Default        | Description                                                    |
|----------------------|:------------------------:|----------------|----------------------------------------------------------------|
| `wordpress_template`      | :heavy_multiplication_x:       |    `twentyseventeen`     | The name of the wordpress template being in use                           |
| `wordpress_import_template` | :heavy_multiplication_x:       | `false`        | If this is true ansible will try to extract a zip file from   `files/{{inventory_hostname}}/{{ wordpress_template }}.zip ` and install it in the wordpress instace |
| `wordpress_stylesheet`      | :heavy_multiplication_x:|        `twentyseventeen`         | What stylesheet should be used for the wordpress instance |

### Import wordpress instance

| Name                 | Required                 | Default        | Description                                                    |
|----------------------|:------------------------:|----------------|----------------------------------------------------------------|
| `wordpress_import_wp_content`      | :heavy_multiplication_x:       |    `false`     | If this is true ansible will try to copy the wp-content directory over to the remote host from   `files/{{inventory_hostname}}/wp-content/`                 |
| `wordpress_import_db_file` | :heavy_multiplication_x:       | `false`        | If this is true ansible will try to import an sql file from `files/{{inventory_hostname}}/wordpress.sql`|

### Mail

| Name                 | Required                 | Default        | Description                                                    |
|----------------------|:------------------------:|----------------|----------------------------------------------------------------|
| `wordpress_mailserver_url`      | :heavy_multiplication_x:       |    `mail.example.com`     | url of the mailserver                           |
| `wordpress_mailserver_login` | :heavy_multiplication_x:       | `login@example.com`        | Login name for the mailserer |
| `wordpress_mailserver_pass`      | :heavy_multiplication_x:|        `password`         | Password for the mailserver|                                  |
| `wordpress_mailserver_port`      | :heavy_multiplication_x:|        `110`         | Mailserver port | 

## Dependencies

The php-fpm role needs to be up and running and a nginx server should be running and serving files from  `/var/www/wordpress` also it needs a maria db instance with the socket running under  `/var/run/mysqld/mysqld.sock`


## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:


### Playbook

```yml
```


### Vars

```yml
```


### Result

Running wordpress instance.

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).


## Author Information

 * [Fritz Otlinghaus(Scriptkiddi)](https://github.com/Scriptkiddi) _fritz.otlinghaus@stuvus.uni-stuttgart.de_
